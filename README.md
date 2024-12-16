import {
  ChangeDetectionStrategy,
  ChangeDetectorRef,
  Component,
  ElementRef,
  EventEmitter,
  forwardRef,
  Input,
  OnDestroy,
  Output,
  SimpleChanges,
  ViewChild,
  ViewEncapsulation
} from '@angular/core';
import {Subscription} from 'rxjs';
import {KeyMoveDirection, RadioGroupService} from './radio-group/radio-group.service';
import {ControlValueAccessor, NG_VALUE_ACCESSOR} from '@angular/forms';

@Component({
  selector: 'finra-radio-internal',
  templateUrl: './radio.component.html',
  styleUrls: ['./radio.component.scss'],
  changeDetection: ChangeDetectionStrategy.OnPush,
  encapsulation: ViewEncapsulation.ShadowDom,
  providers: [{
    provide: NG_VALUE_ACCESSOR,
    useExisting: forwardRef(() => RadioComponent),
    multi: true
  }],
  host: {
    "[attr.aria-checked]": 'checked'
  }
})
export class RadioComponent implements OnDestroy, ControlValueAccessor {
  @Input() checked = false;
  @Input() disabled: boolean;
  @Input() value: string;
  @Input() name: string;
  @Input() readOnly = false;
  radioId = `radio-${Math.floor(Math.random() * 100000000000)}`;

  // tslint:disable-next-line
  @Output() input = new EventEmitter(); // follows standard HTML5 conventions for value change events
  // tslint:disable: no-output-native
  @Output() change = new EventEmitter();
  @Output() click = new EventEmitter();
  @Output() blur = new EventEmitter();
  @Output() focus = new EventEmitter();

  @ViewChild('fakeRadioBtn') fakeCheckBox!: ElementRef<HTMLElement>;

  direction: string;
  private subs: Subscription[] = [];

  constructor(private changeDetectorRef: ChangeDetectorRef, private radioGroupService: RadioGroupService) {

  }

  writeValue(value: string): void {
    this.value = value ? value : '';
  }

  registerOnChange(fn: any): void {
    this.handleChange = fn;
  }

  registerOnTouched(fn: any): void {
    this.handleBlur = fn;
  }

  setDisabledState?(isDisabled: boolean): void {
    this.disabled = isDisabled;
  }


  ngOnChanges(simpleChanges: SimpleChanges) {
    if (simpleChanges.name) {
      this.direction = this.radioGroupService.direction;
      this.ngOnDestroy();
      if (simpleChanges.name.currentValue) {
        this.subs.push(
          this.radioGroupService.focusOnFirstChildWhenValueNull.subscribe(subject => {
            if (subject.id == this.name && subject.value === this.value) {
              this.fakeCheckBox.nativeElement.setAttribute('tabindex', '0');
            }
          })
        );
        this.subs.push(
          this.radioGroupService.change.subscribe( subject => {
            if (subject.id == this.name) {
              this.checked = (subject.value === this.value);
              this.changeDetectorRef.detectChanges();
            }
          })
        );
        this.subs.push(
          this.radioGroupService.focusedRadio.subscribe(subject => {
            if (subject.id === this.name && subject.value === this.value) {
              this.fakeCheckBox.nativeElement.focus();
            }
          })
        );
      }
    }

    this.changeDetectorRef.markForCheck();
  }

  ngOnDestroy() {
    this.subs.forEach(sub => sub && sub.unsubscribe());
  }


  handleClick(event) {
    event.stopPropagation();
    if (this.readOnly) {
      event.preventDefault();
      return;
    }
    this.click.emit(event);
  }

  handleChange(event) {
    event.stopPropagation();
    this.value = event.target.value;
    this.input.emit(this.value);
    this.change.emit(event);

    if (this.name) {
      this.radioGroupService.setRadioGroupValue(this.name, this.value);
    }
  }

  handleBlur(event) {
    event.stopPropagation();
    this.blur.emit(event);
  }

  handleFocus(event) {
    event.stopPropagation();
    this.focus.emit(event);
  }

  handleArrowKey(e: KeyboardEvent) {
    const code = e.code;
    let direction: KeyMoveDirection;
    if (code === 'ArrowRight' || code === 'ArrowDown') {
      direction = "next";
    } else if (code === 'ArrowLeft' || code === 'ArrowUp') {
      direction = "previous";
    }
    if (direction && this.name) {
      e.preventDefault();
      this.radioGroupService.keyMove.next({
        id: this.name,
        value: this.value,
        direction
      });
    }
  }
}
