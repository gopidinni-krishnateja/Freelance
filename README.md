/* Change the outer circle color when the radio button is checked */
.mat-mdc-radio-button .mdc-radio .mdc-radio__native-control:enabled:checked + .mdc-radio__background .mdc-radio__outer-circle {
  border-color: #FF5733 !important;  /* Set outer circle border color */
}

/* Change the inner circle color when the radio button is checked */
.mat-mdc-radio-button .mdc-radio .mdc-radio__native-control:enabled:checked + .mdc-radio__background .mdc-radio__inner-circle {
  background-color: #FF5733 !important;  /* Set inner circle color */
}

/* Change the color of the radio button text */
.mat-mdc-radio-button.mat-radio-checked {
  color: #FF5733 !important; /* Change text color when selected */
}

/* Adjust font size and line height of the radio button text */
.mat-mdc-radio-button {
  font-size: 18px;  /* Adjust text size */
  line-height: 2;   /* Adjust line height for better vertical alignment */
  color: #333;      /* Default text color */
}
