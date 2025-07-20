# License Plate Store - API Documentation

## Table of Contents
1. [Overview](#overview)
2. [Data Models](#data-models)
3. [Services](#services)
4. [Components](#components)
5. [Stores](#stores)
6. [Interceptors](#interceptors)
7. [Configuration](#configuration)
8. [Usage Examples](#usage-examples)

## Overview

This is an Angular application for a license plate store that allows users to browse, add to cart, and purchase license plates. The application includes features like currency switching, authentication, and a shopping cart system.

## Data Models

### LicensePlate Interface

```typescript
export interface LicensePlate {
  _id: string;
  onSale: boolean;
  picture: string;
  title: string;
  price: number;
  year: number;
  state: string;
  description: string;
}
```

**Properties:**
- `_id`: Unique identifier for the license plate
- `onSale`: Boolean indicating if the plate is currently on sale
- `picture`: URL to the license plate image
- `title`: Display name for the license plate
- `price`: Price in the base currency (USD)
- `year`: Year the license plate was issued
- `state`: US state abbreviation (e.g., "CA", "NY")
- `description`: Detailed description of the license plate

### Currency Type

```typescript
export type Currency = 'USD' | 'GBP' | 'EUR';
```

Supported currencies for the application.

## Services

### CartService

**Location:** `src/app/cart.service.ts`

Manages shopping cart operations through HTTP requests to the backend API.

#### Public Methods

##### `getCartContents(): Observable<LicensePlate[]>`
Retrieves the current contents of the user's shopping cart.

**Returns:** Observable that emits an array of LicensePlate objects

**Example:**
```typescript
constructor(private cartService: CartService) {
  this.cartService.getCartContents().subscribe(plates => {
    console.log('Cart contents:', plates);
  });
}
```

##### `addToCart(plate: LicensePlate): Observable<unknown>`
Adds a license plate to the shopping cart.

**Parameters:**
- `plate`: LicensePlate object to add to cart

**Returns:** Observable that completes when the plate is added

**Example:**
```typescript
addToCart(plate: LicensePlate): void {
  this.cartService.addToCart(plate).subscribe(() => {
    console.log('Plate added to cart');
  });
}
```

##### `removeFromCart(plate: LicensePlate): Observable<unknown>`
Removes a license plate from the shopping cart.

**Parameters:**
- `plate`: LicensePlate object to remove from cart

**Returns:** Observable that completes when the plate is removed

**Example:**
```typescript
removeFromCart(plate: LicensePlate): void {
  this.cartService.removeFromCart(plate).subscribe(() => {
    console.log('Plate removed from cart');
  });
}
```

### CurrencyService

**Location:** `src/app/currency.service.ts`

Manages currency conversion and current currency selection.

#### Public Methods

##### `getExchangeRate(): Observable<number>`
Gets the current exchange rate for the selected currency.

**Returns:** Observable that emits the current exchange rate

**Example:**
```typescript
constructor(private currencyService: CurrencyService) {
  this.currencyService.getExchangeRate().subscribe(rate => {
    console.log('Current exchange rate:', rate);
  });
}
```

##### `getCurrency(): Observable<Currency>`
Gets the currently selected currency.

**Returns:** Observable that emits the current currency

**Example:**
```typescript
constructor(private currencyService: CurrencyService) {
  this.currencyService.getCurrency().subscribe(currency => {
    console.log('Current currency:', currency);
  });
}
```

##### `setCurrency(currency: Currency): void`
Sets the current currency and triggers exchange rate updates.

**Parameters:**
- `currency`: The currency to set (USD, GBP, or EUR)

**Example:**
```typescript
changeCurrency(currency: Currency): void {
  this.currencyService.setCurrency(currency);
}
```

### LoginService

**Location:** `src/app/login/login.service.ts`

Handles user authentication and token management.

#### Public Methods

##### `login(username: string, password: string): Observable<string>`
Authenticates a user and returns an authentication token.

**Parameters:**
- `username`: User's username
- `password`: User's password

**Returns:** Observable that emits the authentication token

**Example:**
```typescript
login(username: string, password: string): void {
  this.loginService.login(username, password).subscribe(token => {
    console.log('Login successful, token:', token);
  });
}
```

##### `isUserLoggedIn(): boolean`
Checks if a user is currently logged in.

**Returns:** Boolean indicating login status

**Example:**
```typescript
if (this.loginService.isUserLoggedIn()) {
  console.log('User is logged in');
}
```

##### `getCurrentUser(): string`
Gets the username of the currently logged-in user.

**Returns:** Username string

**Example:**
```typescript
const currentUser = this.loginService.getCurrentUser();
console.log('Current user:', currentUser);
```

##### `getAuthToken(): string`
Gets the current authentication token.

**Returns:** Authentication token string

**Example:**
```typescript
const token = this.loginService.getAuthToken();
console.log('Auth token:', token);
```

## Components

### AppComponent

**Location:** `src/app/app.component.ts`
**Selector:** `app-root`

Root component of the application that contains the navigation and router outlet.

**Template:** `app.component.html`

### StoreViewComponent

**Location:** `src/app/store-view/store-view.component.ts`
**Selector:** `app-store-view`

Displays the main store view with all available license plates.

**Public Properties:**
- `plates$: Observable<LicensePlate[]>`: Observable of all available license plates

**Template:** `store-view.component.html`

**Usage:**
```html
<app-store-view></app-store-view>
```

### LicensePlateComponent

**Location:** `src/app/license-plate/license-plate.component.ts`
**Selector:** `app-license-plate`

Displays individual license plate information with currency conversion.

#### Input Properties

##### `@Input() plate: LicensePlate`
The license plate data to display.

##### `@Input() buttonText: string`
Text to display on the action button.

#### Output Properties

##### `@Output() buttonClicked: EventEmitter<LicensePlate>`
Emitted when the action button is clicked.

**Template:** `license-plate.component.html`

**Usage:**
```html
<app-license-plate 
  [plate]="licensePlate" 
  [buttonText]="'Add to Cart'"
  (buttonClicked)="addToCart($event)">
</app-license-plate>
```

### CartViewComponent

**Location:** `src/app/cart-view/cart-view.component.ts`
**Selector:** `app-cart-view`

Displays the contents of the shopping cart.

**Public Properties:**
- `cartContents: LicensePlate[]`: Array of license plates in the cart

**Public Methods:**
- `removeFromCart(plate: LicensePlate): void`: Removes a plate from the cart

**Template:** `cart-view.component.html`

**Usage:**
```html
<app-cart-view></app-cart-view>
```

### CheckoutViewComponent

**Location:** `src/app/checkout-view/checkout-view.component.ts`
**Selector:** `app-checkout-view`

Displays the checkout page with the checkout form.

**Template:** `checkout-view.component.html`

**Usage:**
```html
<app-checkout-view></app-checkout-view>
```

### CheckoutFormComponent

**Location:** `src/app/checkout-form/checkout-form.component.ts`
**Selector:** `app-checkout-form`

Handles the checkout form submission.

**Public Methods:**
- `logForm(value: object): void`: Logs form values to console

**Template:** `checkout-form.component.html`

**Usage:**
```html
<app-checkout-form></app-checkout-form>
```

### CurrencySwitcherComponent

**Location:** `src/app/currency-switcher/currency-switcher.component.ts`
**Selector:** `app-currency-switcher`

Allows users to switch between different currencies.

**Public Properties:**
- `showItems: boolean`: Controls visibility of currency dropdown
- `currency$: Observable<Currency>`: Current selected currency

**Public Methods:**
- `changeCurrency(currency: Currency): void`: Changes the current currency

**Template:** `currency-switcher.component.html`

**Usage:**
```html
<app-currency-switcher></app-currency-switcher>
```

### NavigationComponent

**Location:** `src/app/navigation/navigation.component.ts`
**Selector:** `app-navigation`

Main navigation component with links and currency switcher.

**Template:** `navigation.component.html`

**Usage:**
```html
<app-navigation></app-navigation>
```

### JumbotronComponent

**Location:** `src/app/jumbotron/jumbotron.component.ts`
**Selector:** `app-jumbotron`

Displays a jumbotron header with title and description.

#### Input Properties

##### `@Input() title: string`
The title to display in the jumbotron.

##### `@Input() description: string`
The description to display in the jumbotron.

**Template:** `jumbotron.component.html`

**Usage:**
```html
<app-jumbotron 
  title="Welcome to License Plate Store" 
  description="Find your perfect license plate">
</app-jumbotron>
```

### DialogComponent

**Location:** `src/app/dialog/dialog.component.ts`
**Selector:** `app-dialog`
**Standalone:** Yes

A reusable dialog/modal component.

#### Input Properties

##### `@Input() isOpen: boolean = false`
Controls whether the dialog is visible.

##### `@Input() title: string = "Title"`
The title displayed in the dialog header.

#### Output Properties

##### `@Output() onClose: EventEmitter<string>`
Emitted when the dialog is closed.

**Public Methods:**
- `closePopup(): void`: Closes the dialog and emits onClose event

**Template:** `dialog.component.html`

**Usage:**
```html
<app-dialog 
  [isOpen]="showDialog" 
  [title]="'Confirmation'"
  (onClose)="handleDialogClose($event)">
</app-dialog>
```

### LoginComponent

**Location:** `src/app/login/login.component.ts`
**Selector:** `app-login`
**Standalone:** Yes

Handles user login functionality.

**Public Methods:**
- `login(username: string, password: string): void`: Performs user login

**Template:** `login.component.html`

**Usage:**
```html
<app-login></app-login>
```

## Stores

### Store<T> (Base Class)

**Location:** `src/app/store/store.ts`

Generic store class for state management using RxJS BehaviorSubject.

#### Public Properties

##### `state$: Observable<T>`
Observable of the current state.

#### Public Methods

##### `get state(): T`
Gets the current state value.

##### `setState(nextState: T): void`
Updates the state with a new value.

**Example:**
```typescript
class MyStore extends Store<string[]> {
  constructor() {
    super([]); // Initialize with empty array
  }
}

const store = new MyStore();
store.state$.subscribe(state => console.log('State:', state));
store.setState(['item1', 'item2']);
```

### LicensePlateStore

**Location:** `src/app/store/plate.store.ts`
**Extends:** `Store<LicensePlate[]>`

Manages license plate data with HTTP integration.

#### Public Methods

##### `addPlate(plate: LicensePlate): void`
Adds a license plate to the store.

**Parameters:**
- `plate`: LicensePlate object to add

##### `getPlates(): Observable<LicensePlate[]>`
Gets observable of all license plates.

**Returns:** Observable of LicensePlate array

##### `refresh(): void`
Refreshes the store data from the server.

**Example:**
```typescript
constructor(private plateStore: LicensePlateStore) {
  this.plateStore.getPlates().subscribe(plates => {
    console.log('Available plates:', plates);
  });
}
```

## Interceptors

### TokenInterceptorService

**Location:** `src/app/token-http-interceptor.service.ts`

HTTP interceptor that automatically adds authentication tokens to requests.

**Implements:** `HttpInterceptor`

#### Public Methods

##### `intercept(req: HttpRequest<unknown>, next: HttpHandler): Observable<HttpEvent<unknown>>`
Intercepts HTTP requests and adds authentication token if available.

**Parameters:**
- `req`: The HTTP request to intercept
- `next`: The next handler in the chain

**Returns:** Observable of HTTP events

**Usage:**
This interceptor is automatically applied to all HTTP requests when the service is provided in the application configuration.

## Configuration

### App Configuration

**Location:** `src/app/app.config.ts`

Main application configuration with providers.

**Exports:**
- `appConfig: ApplicationConfig`: Application configuration object

**Providers:**
- Router provider for navigation
- HTTP client provider for API requests

### Routes Configuration

**Location:** `src/app/app.routes.ts`

Application routing configuration.

**Routes:**
- `""` → `StoreViewComponent` (home page)
- `"cart"` → `CartViewComponent` (shopping cart)
- `"checkout"` → `CheckoutViewComponent` (checkout page)

## Usage Examples

### Basic Component Usage

```typescript
// In a component
import { Component } from '@angular/core';
import { CartService } from './cart.service';
import { CurrencyService } from './currency.service';
import { LicensePlate } from './license-plate';

@Component({
  selector: 'app-example',
  template: `
    <div *ngFor="let plate of plates$ | async">
      <app-license-plate 
        [plate]="plate" 
        [buttonText]="'Add to Cart'"
        (buttonClicked)="addToCart($event)">
      </app-license-plate>
    </div>
    <app-currency-switcher></app-currency-switcher>
  `
})
export class ExampleComponent {
  plates$ = this.http.get<LicensePlate[]>('https://lp-store-server.vercel.app/data');
  
  constructor(
    private cartService: CartService,
    private currencyService: CurrencyService,
    private http: HttpClient
  ) {}
  
  addToCart(plate: LicensePlate): void {
    this.cartService.addToCart(plate).subscribe(() => {
      console.log('Added to cart');
    });
  }
}
```

### Service Integration

```typescript
// Using multiple services together
export class MyComponent {
  constructor(
    private cartService: CartService,
    private currencyService: CurrencyService,
    private loginService: LoginService
  ) {
    // Subscribe to cart contents
    this.cartService.getCartContents().subscribe(plates => {
      console.log('Cart has', plates.length, 'items');
    });
    
    // Subscribe to currency changes
    this.currencyService.getCurrency().subscribe(currency => {
      console.log('Currency changed to:', currency);
    });
  }
  
  login(): void {
    this.loginService.login('username', 'password').subscribe(token => {
      console.log('Logged in with token:', token);
    });
  }
}
```

### Store Usage

```typescript
// Using the license plate store
export class StoreComponent {
  plates$ = this.plateStore.getPlates();
  
  constructor(private plateStore: LicensePlateStore) {}
  
  addNewPlate(plate: LicensePlate): void {
    this.plateStore.addPlate(plate);
  }
  
  refreshData(): void {
    this.plateStore.refresh();
  }
}
```

### Dialog Usage

```typescript
// Using the dialog component
export class DialogExampleComponent {
  showDialog = false;
  
  openDialog(): void {
    this.showDialog = true;
  }
  
  handleDialogClose(message: string): void {
    this.showDialog = false;
    console.log('Dialog closed:', message);
  }
}
```

## API Endpoints

The application communicates with the following endpoints:

- `https://lp-store-server.vercel.app/data` - Get all license plates
- `https://lp-store-server.vercel.app/rates` - Get exchange rates
- `http://localhost:8000/cart` - Cart operations (GET, PUT, DELETE)
- `http://localhost:8000/login` - Authentication (PUT)

## Dependencies

### Core Dependencies
- Angular 19.1.3 (core, common, forms, router, etc.)
- RxJS 7.8.0 - Reactive programming
- Bootstrap 4.6 - UI framework
- lp-store-server 2.3.2 - Backend API

### Development Dependencies
- Angular CLI 19.1.4
- TypeScript 5.7.3
- Jasmine & Karma - Testing framework

## Getting Started

1. Install dependencies:
   ```bash
   npm install
   ```

2. Start the development server:
   ```bash
   npm start
   ```

3. Build for production:
   ```bash
   npm run build
   ```

4. Run tests:
   ```bash
   npm test
   ```

## Notes

- The application uses Angular's standalone components and modern dependency injection patterns
- HTTP requests are automatically intercepted to add authentication tokens
- Currency conversion is handled automatically through the CurrencyService
- The application follows Angular best practices for component architecture and state management