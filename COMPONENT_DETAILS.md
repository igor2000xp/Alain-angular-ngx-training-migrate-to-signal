# Component Details and Templates

This document provides detailed information about each component's template, styling, and implementation details.

## AppComponent

**Template:** `src/app/app.component.html`
```html
<app-navigation></app-navigation>
<router-outlet></router-outlet>
```

**Description:** Root component that provides the main layout with navigation and router outlet for dynamic content.

## StoreViewComponent

**Template:** `src/app/store-view/store-view.component.html`
```html
<app-jumbotron title="License Plates" description="Browse our collection of license plates"></app-jumbotron>
<div class="container">
  <div class="row">
    <div class="col-md-4" *ngFor="let plate of plates$ | async">
      <app-license-plate [plate]="plate" [buttonText]="'Add to Cart'" (buttonClicked)="addToCart($event)"></app-license-plate>
    </div>
  </div>
</div>
```

**Description:** Main store view that displays all available license plates in a grid layout using Bootstrap classes.

**Styling:** `src/app/store-view/store-view.component.css` (empty - uses Bootstrap classes)

## LicensePlateComponent

**Template:** `src/app/license-plate/license-plate.component.html`
```html
<div class="card">
  <img [src]="plate.picture" class="card-img-top" [alt]="plate.title">
  <div class="card-body">
    <h5 class="card-title">{{ plate.title }}</h5>
    <p class="card-text">{{ plate.description }}</p>
    <p class="card-text">
      <small class="text-muted">Year: {{ plate.year }} | State: {{ plate.state }}</small>
    </p>
    <p class="card-text">
      <strong>Price: {{ plate.price | currency:(currency$ | async) }}</strong>
    </p>
    <button class="btn btn-primary" (click)="buttonClicked.emit(plate)">
      {{ buttonText }}
    </button>
  </div>
</div>
```

**Styling:** `src/app/license-plate/license-plate.component.css`
```css
.card {
  margin-bottom: 20px;
}

.card-img-top {
  height: 200px;
  object-fit: cover;
}
```

**Description:** Displays individual license plate information in a Bootstrap card format with currency conversion.

## CartViewComponent

**Template:** `src/app/cart-view/cart-view.component.html`
```html
<app-jumbotron title="Shopping Cart" description="Your selected license plates"></app-jumbotron>
<div class="container">
  <div *ngIf="cartContents.length === 0" class="alert alert-info">
    Your cart is empty.
  </div>
  <div class="row" *ngIf="cartContents.length > 0">
    <div class="col-md-4" *ngFor="let plate of cartContents">
      <app-license-plate [plate]="plate" [buttonText]="'Remove'" (buttonClicked)="removeFromCart($event)"></app-license-plate>
    </div>
  </div>
</div>
```

**Description:** Displays cart contents with option to remove items. Shows empty cart message when no items are present.

## CheckoutViewComponent

**Template:** `src/app/checkout-view/checkout-view.component.html`
```html
<app-jumbotron title="Checkout" description="Complete your purchase"></app-jumbotron>
<div class="container">
  <app-checkout-form></app-checkout-form>
</div>
```

**Description:** Checkout page that contains the checkout form component.

## CheckoutFormComponent

**Template:** `src/app/checkout-form/checkout-form.component.html`
```html
<form #checkoutForm="ngForm" (ngSubmit)="logForm(checkoutForm.value)">
  <div class="form-group">
    <label for="firstName">First Name</label>
    <input type="text" class="form-control" id="firstName" name="firstName" ngModel required>
  </div>
  
  <div class="form-group">
    <label for="lastName">Last Name</label>
    <input type="text" class="form-control" id="lastName" name="lastName" ngModel required>
  </div>
  
  <div class="form-group">
    <label for="email">Email</label>
    <input type="email" class="form-control" id="email" name="email" ngModel required>
  </div>
  
  <div class="form-group">
    <label for="address">Address</label>
    <textarea class="form-control" id="address" name="address" rows="3" ngModel required></textarea>
  </div>
  
  <div class="form-group">
    <label for="city">City</label>
    <input type="text" class="form-control" id="city" name="city" ngModel required>
  </div>
  
  <div class="form-group">
    <label for="state">State</label>
    <select class="form-control" id="state" name="state" ngModel required>
      <option value="">Select State</option>
      <option value="AL">Alabama</option>
      <option value="AK">Alaska</option>
      <!-- More states... -->
    </select>
  </div>
  
  <div class="form-group">
    <label for="zipCode">ZIP Code</label>
    <input type="text" class="form-control" id="zipCode" name="zipCode" ngModel required>
  </div>
  
  <button type="submit" class="btn btn-primary">Complete Purchase</button>
</form>
```

**Styling:** `src/app/checkout-form/checkout-form.component.css`
```css
.form-group {
  margin-bottom: 1rem;
}

.form-control {
  display: block;
  width: 100%;
  padding: 0.375rem 0.75rem;
  font-size: 1rem;
  line-height: 1.5;
  color: #495057;
  background-color: #fff;
  border: 1px solid #ced4da;
  border-radius: 0.25rem;
}

.btn-primary {
  color: #fff;
  background-color: #007bff;
  border-color: #007bff;
}
```

**Description:** Comprehensive checkout form with customer information fields using Angular template-driven forms.

## CurrencySwitcherComponent

**Template:** `src/app/currency-switcher/currency-switcher.component.html`
```html
<div class="dropdown">
  <button class="btn btn-secondary dropdown-toggle" type="button" (click)="showItems = !showItems">
    {{ currency$ | async }}
  </button>
  <div class="dropdown-menu" [class.show]="showItems">
    <a class="dropdown-item" href="#" (click)="changeCurrency('USD')">USD</a>
    <a class="dropdown-item" href="#" (click)="changeCurrency('EUR')">EUR</a>
    <a class="dropdown-item" href="#" (click)="changeCurrency('GBP')">GBP</a>
  </div>
</div>
```

**Styling:** `src/app/currency-switcher/currency-switcher.component.css`
```css
.dropdown {
  position: relative;
  display: inline-block;
}

.dropdown-menu {
  position: absolute;
  top: 100%;
  left: 0;
  z-index: 1000;
  display: none;
  min-width: 10rem;
  padding: 0.5rem 0;
  margin: 0.125rem 0 0;
  background-color: #fff;
  border: 1px solid rgba(0,0,0,.15);
  border-radius: 0.25rem;
}

.dropdown-menu.show {
  display: block;
}
```

**Description:** Dropdown component for currency selection with Bootstrap styling.

## NavigationComponent

**Template:** `src/app/navigation/navigation.component.html`
```html
<nav class="navbar navbar-expand-lg navbar-dark bg-dark">
  <a class="navbar-brand" href="#">License Plate Store</a>
  
  <div class="collapse navbar-collapse">
    <ul class="navbar-nav mr-auto">
      <li class="nav-item">
        <a class="nav-link" routerLink="/">Store</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" routerLink="/cart">Cart</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" routerLink="/checkout">Checkout</a>
      </li>
    </ul>
    
    <div class="navbar-nav ml-auto">
      <app-currency-switcher></app-currency-switcher>
    </div>
  </div>
</nav>
```

**Description:** Main navigation bar with links to different sections and currency switcher.

## JumbotronComponent

**Template:** `src/app/jumbotron/jumbotron.component.html`
```html
<div class="jumbotron">
  <h1 class="display-4">{{ title }}</h1>
  <p class="lead">{{ description }}</p>
</div>
```

**Description:** Bootstrap jumbotron component for page headers with customizable title and description.

## DialogComponent

**Template:** `src/app/dialog/dialog.component.html`
```html
<div class="modal" [class.show]="isOpen" [style.display]="isOpen ? 'block' : 'none'">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title">{{ title }}</h5>
        <button type="button" class="close" (click)="closePopup()">
          <span>&times;</span>
        </button>
      </div>
      <div class="modal-body">
        <ng-content></ng-content>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" (click)="closePopup()">Close</button>
      </div>
    </div>
  </div>
</div>
<div class="modal-backdrop fade show" *ngIf="isOpen"></div>
```

**Styling:** `src/app/dialog/dialog.component.css`
```css
.modal {
  position: fixed;
  top: 0;
  left: 0;
  z-index: 1050;
  width: 100%;
  height: 100%;
  overflow: hidden;
  outline: 0;
}

.modal-dialog {
  position: relative;
  width: auto;
  margin: 0.5rem;
  pointer-events: none;
}

.modal-content {
  position: relative;
  display: flex;
  flex-direction: column;
  width: 100%;
  pointer-events: auto;
  background-color: #fff;
  border: 1px solid rgba(0,0,0,.2);
  border-radius: 0.3rem;
  outline: 0;
}

.modal-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  z-index: 1040;
  width: 100vw;
  height: 100vh;
  background-color: #000;
  opacity: 0.5;
}
```

**Description:** Reusable modal dialog component with backdrop and customizable content.

## LoginComponent

**Template:** `src/app/login/login.component.html`
```html
<div class="login-container">
  <h2>Login</h2>
  <form (ngSubmit)="login(username.value, password.value)">
    <div class="form-group">
      <label for="username">Username</label>
      <input type="text" class="form-control" id="username" #username required>
    </div>
    <div class="form-group">
      <label for="password">Password</label>
      <input type="password" class="form-control" id="password" #password required>
    </div>
    <button type="submit" class="btn btn-primary">Login</button>
  </form>
</div>
```

**Styling:** `src/app/login/login.component.css`
```css
.login-container {
  max-width: 400px;
  margin: 50px auto;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 5px;
}

.form-group {
  margin-bottom: 15px;
}

.form-control {
  width: 100%;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
}
```

**Description:** Login form component with username and password fields.

## Component Communication Patterns

### Parent-Child Communication

**Input Properties:**
```typescript
// Parent component
<app-license-plate [plate]="licensePlate" [buttonText]="'Add to Cart'"></app-license-plate>

// Child component
@Input() plate!: LicensePlate;
@Input() buttonText!: string;
```

**Output Events:**
```typescript
// Child component
@Output() buttonClicked = new EventEmitter<LicensePlate>();

// Parent component
<app-license-plate (buttonClicked)="handleButtonClick($event)"></app-license-plate>
```

### Service-Based Communication

**Shared State:**
```typescript
// Service
@Injectable({ providedIn: 'root' })
export class CartService {
  private cartItems$ = new BehaviorSubject<LicensePlate[]>([]);
  
  getCartItems() {
    return this.cartItems$.asObservable();
  }
}

// Components
constructor(private cartService: CartService) {
  this.cartService.getCartItems().subscribe(items => {
    this.cartItems = items;
  });
}
```

## Styling Guidelines

### Bootstrap Integration
- All components use Bootstrap 4.6 classes for consistent styling
- Responsive design using Bootstrap grid system
- Form styling follows Bootstrap form patterns

### Custom CSS
- Component-specific styles are kept minimal
- Focus on layout and spacing adjustments
- Use Bootstrap utilities when possible

### Responsive Design
- Mobile-first approach with Bootstrap breakpoints
- Grid system adapts to different screen sizes
- Navigation collapses on smaller screens

## Template Best Practices

### Structural Directives
```html
<!-- *ngFor for lists -->
<div *ngFor="let item of items; trackBy: trackByFn">
  {{ item.name }}
</div>

<!-- *ngIf for conditional rendering -->
<div *ngIf="isVisible">
  Content to show
</div>

<!-- *ngSwitch for multiple conditions -->
<div [ngSwitch]="status">
  <div *ngSwitchCase="'active'">Active</div>
  <div *ngSwitchCase="'inactive'">Inactive</div>
  <div *ngSwitchDefault>Unknown</div>
</div>
```

### Event Binding
```html
<!-- Click events -->
<button (click)="handleClick()">Click me</button>

<!-- Form events -->
<form (ngSubmit)="onSubmit()">
  <input (input)="onInput($event)">
</form>

<!-- Custom events -->
<app-child (customEvent)="handleCustomEvent($event)"></app-child>
```

### Property Binding
```html
<!-- Attribute binding -->
<img [src]="imageUrl" [alt]="imageAlt">

<!-- Class binding -->
<div [class.active]="isActive" [class.disabled]="isDisabled">

<!-- Style binding -->
<div [style.width]="width" [style.height]="height">
```

## Performance Considerations

### OnPush Change Detection
```typescript
@Component({
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class OptimizedComponent {
  // Component with optimized change detection
}
```

### TrackBy Functions
```typescript
trackByFn(index: number, item: any): any {
  return item.id; // or item._id
}
```

### Async Pipe Usage
```html
<!-- Prevents memory leaks -->
<div *ngFor="let item of items$ | async">
  {{ item.name }}
</div>
```

## Testing Considerations

### Component Testing
```typescript
describe('LicensePlateComponent', () => {
  let component: LicensePlateComponent;
  let fixture: ComponentFixture<LicensePlateComponent>;

  beforeEach(() => {
    TestBed.configureTestingModule({
      declarations: [LicensePlateComponent]
    });
    fixture = TestBed.createComponent(LicensePlateComponent);
    component = fixture.componentInstance;
  });

  it('should emit buttonClicked event', () => {
    spyOn(component.buttonClicked, 'emit');
    component.plate = mockPlate;
    fixture.detectChanges();
    
    const button = fixture.nativeElement.querySelector('button');
    button.click();
    
    expect(component.buttonClicked.emit).toHaveBeenCalledWith(mockPlate);
  });
});
```

### Service Testing
```typescript
describe('CartService', () => {
  let service: CartService;
  let httpMock: HttpTestingController;

  beforeEach(() => {
    TestBed.configureTestingModule({
      imports: [HttpClientTestingModule],
      providers: [CartService]
    });
    service = TestBed.inject(CartService);
    httpMock = TestBed.inject(HttpTestingController);
  });

  it('should get cart contents', () => {
    const mockPlates = [mockPlate];
    
    service.getCartContents().subscribe(plates => {
      expect(plates).toEqual(mockPlates);
    });

    const req = httpMock.expectOne('http://localhost:8000/cart');
    expect(req.request.method).toBe('GET');
    req.flush(mockPlates);
  });
});
```