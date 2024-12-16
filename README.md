<ng-container *ngFor="let tidType of tidTypeOptions">
                    <div class="pt-1">
                        <finra-radio name="tidType" 
                        slot="item"
                        value="tidType.value"  
                        inputId="tidType.value"
                        formControlName="tidType" 
                        [id]="'tidType' + tidType.value"
                      >{{tidType.label}}</finra-radio>

                
                    </div>
                </ng-container>
