 // No value selected initially
  selectedValue: string | null = null;  // Set to null to ensure no selection by default
  selectedText: string | null = null;   // Set to null to ensure no selection by default

  // Method to handle the execution of the command for a selected option
  handleCommand(option: string): void {
    console.log('Executing command for:', option);
    this.selectedText = option;  // Update the selected text when a command is executed
    this.selectedValue = option; // Update the selected value
    // You can add further logic here, like toggling the dropdown or other UI updates
  }


  <finra-dropdown [label]="'Label Text'"
                [selectedvalue]="selectedValue"
                [selectedtext]="selectedText"
                [style]="'width: 100px'">
  <ng-container *ngFor="let option of options">
    <finra-dropdown-option 
      slot="option" 
      [value]="option.label"
      (click)="option.command()">
      {{ option.label }}
    </finra-dropdown-option>
  </ng-container>
</finra-dropdown>
