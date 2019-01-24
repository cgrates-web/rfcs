# Progress indecator for resolve raw suppliers rates

## Motivation

When a user runs resolve process on the "raw suppliers rates" page there is not any notification about a finish.
To actualize the state the user should refresh the page. This is not convenient if the process takes quite a long time.

## Detailed design

### Backend
1. Need to implement a GenServer for the resolve process with 2 functions async - run, sync - get_state.
The run function should start the resolve process (calls `CgratesWebJsonapi.RawSupplierRate.Resolver.resolve/1`).
The GenServer should restart the process automatically after fail.
2. Need to implement a registry to manege different pocesses for different tariff_plans. 
3. Need to implement a supervisor.
4. Need to implement notification via web sockets about progress. 

### Frontend
1. Reimplement resolve action from tariff-plan/raw-supplier-rate/index controller as a task from ember-concurrency.
2. Impement progress indecator on the tariff-plan/raw-supplier-rates page.
3. If resolve task still has in progress state all edit/new/destroy/upload/resolve buttons should be disabled.
4. Use WebSockets to connect to the backend and to receive progress details
