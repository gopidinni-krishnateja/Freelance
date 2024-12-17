<finra-switch [(ngModel)]="queryParameters.authTraderNames.exactMatch" (input)="togglAuthTraderSwitch(queryParameters.authTraderNames.exactMatch,$event)">Exact match</finra-switch>
            <div>{{queryParameters.authTraderNames.exactMatch}}</div>
