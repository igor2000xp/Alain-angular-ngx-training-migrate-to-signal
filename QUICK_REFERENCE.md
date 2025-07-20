# Quick Reference Guide

## Common Patterns

### Service Injection
```typescript
constructor(
  private cartService: CartService,
  private currencyService: CurrencyService,
  private http: HttpClient
) {}
```

### Observable Subscription
```typescript
// In constructor
this.data$ = this.service.getData();

// In template
<div *ngFor="let item of data$ | async">
  {{ item.name }}
</div>
```

### HTTP Requests
```typescript
// GET request
this.http.get<Type[]>('url').subscribe(data => {
  this.items = data;
});

// POST request
this.http.post<Response>('url', body).subscribe(response => {
  console.log(response);
});

// PUT request
this.http.put<Response>('url', body).subscribe(response => {
  console.log(response);
});

// DELETE request
this.http.delete('url').subscribe(() => {
  console.log('Deleted');
});
```

### Component Input/Output
```typescript
// Input property
@Input() data!: Type;

// Output event
@Output() eventName = new EventEmitter<Type>();

// Emit event
this.eventName.emit(value);
```

### Template Forms
```typescript
// Template reference
<form #form="ngForm" (ngSubmit)="onSubmit(form.value)">
  <input name="field" ngModel required>
</form>

// Component method
onSubmit(formData: any): void {
  console.log(formData);
}
```

## Component Selectors

| Component | Selector | Location |
|-----------|----------|----------|
| AppComponent | `app-root` | `src/app/app.component.ts` |
| StoreViewComponent | `app-store-view` | `src/app/store-view/store-view.component.ts` |
| LicensePlateComponent | `app-license-plate` | `src/app/license-plate/license-plate.component.ts` |
| CartViewComponent | `app-cart-view` | `src/app/cart-view/cart-view.component.ts` |
| CheckoutViewComponent | `app-checkout-view` | `src/app/checkout-view/checkout-view.component.ts` |
| CheckoutFormComponent | `app-checkout-form` | `src/app/checkout-form/checkout-form.component.ts` |
| CurrencySwitcherComponent | `app-currency-switcher` | `src/app/currency-switcher/currency-switcher.component.ts` |
| NavigationComponent | `app-navigation` | `src/app/navigation/navigation.component.ts` |
| JumbotronComponent | `app-jumbotron` | `src/app/jumbotron/jumbotron.component.ts` |
| DialogComponent | `app-dialog` | `src/app/dialog/dialog.component.ts` |
| LoginComponent | `app-login` | `src/app/login/login.component.ts` |

## Service Methods

### CartService
```typescript
getCartContents(): Observable<LicensePlate[]>
addToCart(plate: LicensePlate): Observable<unknown>
removeFromCart(plate: LicensePlate): Observable<unknown>
```

### CurrencyService
```typescript
getExchangeRate(): Observable<number>
getCurrency(): Observable<Currency>
setCurrency(currency: Currency): void
```

### LoginService
```typescript
login(username: string, password: string): Observable<string>
isUserLoggedIn(): boolean
getCurrentUser(): string
getAuthToken(): string
```

### LicensePlateStore
```typescript
addPlate(plate: LicensePlate): void
getPlates(): Observable<LicensePlate[]>
refresh(): void
```

## Data Models

### LicensePlate
```typescript
interface LicensePlate {
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

### Currency
```typescript
type Currency = 'USD' | 'GBP' | 'EUR';
```

## Routes

| Path | Component | Description |
|------|-----------|-------------|
| `/` | StoreViewComponent | Home page with license plates |
| `/cart` | CartViewComponent | Shopping cart |
| `/checkout` | CheckoutViewComponent | Checkout page |

## API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `https://lp-store-server.vercel.app/data` | GET | Get all license plates |
| `https://lp-store-server.vercel.app/rates` | GET | Get exchange rates |
| `http://localhost:8000/cart` | GET | Get cart contents |
| `http://localhost:8000/cart/:id` | PUT | Add item to cart |
| `http://localhost:8000/cart/:id` | DELETE | Remove item from cart |
| `http://localhost:8000/login` | PUT | User authentication |

## Bootstrap Classes

### Grid System
```html
<div class="container">
  <div class="row">
    <div class="col-md-4">Column 1</div>
    <div class="col-md-4">Column 2</div>
    <div class="col-md-4">Column 3</div>
  </div>
</div>
```

### Components
```html
<!-- Cards -->
<div class="card">
  <div class="card-body">
    <h5 class="card-title">Title</h5>
    <p class="card-text">Content</p>
  </div>
</div>

<!-- Buttons -->
<button class="btn btn-primary">Primary</button>
<button class="btn btn-secondary">Secondary</button>

<!-- Forms -->
<div class="form-group">
  <label for="input">Label</label>
  <input type="text" class="form-control" id="input">
</div>

<!-- Navigation -->
<nav class="navbar navbar-expand-lg navbar-dark bg-dark">
  <a class="navbar-brand">Brand</a>
  <ul class="navbar-nav">
    <li class="nav-item">
      <a class="nav-link">Link</a>
    </li>
  </ul>
</nav>
```

## Angular Pipes

### Built-in Pipes
```html
<!-- Currency -->
{{ price | currency:'USD' }}

<!-- Async -->
<div *ngFor="let item of items$ | async">
  {{ item.name }}
</div>

<!-- Date -->
{{ date | date:'short' }}

<!-- Uppercase/Lowercase -->
{{ text | uppercase }}
{{ text | lowercase }}
```

## Common Imports

### Angular Core
```typescript
import { Component, Input, Output, EventEmitter } from '@angular/core';
import { Injectable } from '@angular/core';
import { OnInit, OnDestroy } from '@angular/core';
```

### Angular Common
```typescript
import { AsyncPipe, CurrencyPipe } from '@angular/common';
import { NgIf, NgFor } from '@angular/common';
```

### Angular Forms
```typescript
import { FormsModule } from '@angular/forms';
import { ReactiveFormsModule } from '@angular/forms';
```

### Angular Router
```typescript
import { RouterLink, RouterOutlet } from '@angular/router';
import { ActivatedRoute } from '@angular/router';
```

### HTTP
```typescript
import { HttpClient } from '@angular/common/http';
import { HttpInterceptor, HttpRequest, HttpHandler } from '@angular/common/http';
```

### RxJS
```typescript
import { Observable, BehaviorSubject } from 'rxjs';
import { map, tap, switchMap } from 'rxjs/operators';
```

## Error Handling

### HTTP Error Handling
```typescript
this.http.get<Data>('url').pipe(
  catchError(error => {
    console.error('Error:', error);
    return throwError(() => error);
  })
).subscribe(data => {
  // Handle success
});
```

### Observable Error Handling
```typescript
this.service.getData().subscribe({
  next: (data) => {
    // Handle success
  },
  error: (error) => {
    // Handle error
  },
  complete: () => {
    // Handle completion
  }
});
```

## Testing Patterns

### Component Testing Setup
```typescript
describe('ComponentName', () => {
  let component: ComponentName;
  let fixture: ComponentFixture<ComponentName>;

  beforeEach(() => {
    TestBed.configureTestingModule({
      declarations: [ComponentName],
      imports: [FormsModule, HttpClientTestingModule]
    });
    fixture = TestBed.createComponent(ComponentName);
    component = fixture.componentInstance;
  });

  it('should create', () => {
    expect(component).toBeTruthy();
  });
});
```

### Service Testing Setup
```typescript
describe('ServiceName', () => {
  let service: ServiceName;
  let httpMock: HttpTestingController;

  beforeEach(() => {
    TestBed.configureTestingModule({
      imports: [HttpClientTestingModule],
      providers: [ServiceName]
    });
    service = TestBed.inject(ServiceName);
    httpMock = TestBed.inject(HttpTestingController);
  });

  afterEach(() => {
    httpMock.verify();
  });
});
```

## Performance Tips

### OnPush Change Detection
```typescript
@Component({
  changeDetection: ChangeDetectionStrategy.OnPush
})
```

### TrackBy Function
```typescript
trackByFn(index: number, item: any): any {
  return item.id;
}
```

### Async Pipe (prevents memory leaks)
```html
<div *ngFor="let item of items$ | async">
  {{ item.name }}
</div>
```

### Unsubscribe from Observables
```typescript
private destroy$ = new Subject<void>();

ngOnInit() {
  this.data$.pipe(
    takeUntil(this.destroy$)
  ).subscribe(data => {
    // Handle data
  });
}

ngOnDestroy() {
  this.destroy$.next();
  this.destroy$.complete();
}
```

## Common Issues & Solutions

### Template Binding Issues
```typescript
// Use safe navigation operator
{{ user?.name }}

// Use non-null assertion when you're sure
{{ user!.name }}

// Use default values
{{ user?.name || 'Unknown' }}
```

### Async Data Loading
```typescript
// Show loading state
<div *ngIf="data$ | async as data; else loading">
  {{ data.name }}
</div>
<ng-template #loading>
  <div>Loading...</div>
</ng-template>
```

### Form Validation
```typescript
// Template-driven forms
<input name="email" ngModel required email #email="ngModel">
<div *ngIf="email.invalid && email.touched">
  <div *ngIf="email.errors?.['required']">Email is required</div>
  <div *ngIf="email.errors?.['email']">Invalid email format</div>
</div>
```

## Development Commands

```bash
# Install dependencies
npm install

# Start development server
npm start

# Build for production
npm run build

# Run tests
npm test

# Serve built application
npm run serve-build-static

# Clean up
npm run cleanup
```