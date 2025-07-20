# License Plate Store - Angular Application

A comprehensive Angular application for browsing and purchasing license plates with features like currency conversion, shopping cart, and user authentication.

## 📚 Documentation

This project includes comprehensive documentation:

- **[API Documentation](API_DOCUMENTATION.md)** - Complete API reference for all services, components, and data models
- **[Component Details](COMPONENT_DETAILS.md)** - Detailed component templates, styling, and implementation patterns
- **[Quick Reference](QUICK_REFERENCE.md)** - Developer quick reference guide with common patterns and examples

## 🚀 Features

- **License Plate Browsing** - View all available license plates with images and details
- **Shopping Cart** - Add and remove items from cart
- **Currency Conversion** - Switch between USD, EUR, and GBP with real-time conversion
- **User Authentication** - Login system with token-based authentication
- **Responsive Design** - Mobile-first design using Bootstrap 4.6
- **Checkout Process** - Complete checkout form with customer information

## 🛠️ Technology Stack

- **Angular 19.1.3** - Latest Angular framework
- **TypeScript 5.7.3** - Type-safe JavaScript
- **RxJS 7.8.0** - Reactive programming
- **Bootstrap 4.6** - UI framework
- **Angular Router** - Client-side routing
- **Angular Forms** - Template-driven forms
- **HTTP Client** - API communication

## 📁 Project Structure

```
src/
├── app/
│   ├── cart-view/          # Shopping cart component
│   ├── checkout-form/      # Checkout form component
│   ├── checkout-view/      # Checkout page component
│   ├── currency-switcher/  # Currency selection component
│   ├── dialog/            # Reusable dialog component
│   ├── hello/             # Hello world component
│   ├── jumbotron/         # Page header component
│   ├── license-plate/     # Individual plate component
│   ├── login/             # Login component and service
│   ├── navigation/        # Main navigation component
│   ├── store/             # State management stores
│   ├── store-view/        # Main store view component
│   ├── app.component.ts   # Root component
│   ├── app.config.ts      # Application configuration
│   ├── app.routes.ts      # Routing configuration
│   ├── cart.service.ts    # Shopping cart service
│   ├── currency.service.ts # Currency conversion service
│   ├── license-plate.ts   # License plate interface
│   ├── mock-data.ts       # Sample data
│   └── token-http-interceptor.service.ts # HTTP interceptor
├── assets/                # Static assets
└── data/                  # JSON data files
```

## 🚀 Getting Started

### Prerequisites

- Node.js (version 16 or higher)
- npm (comes with Node.js)

### Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd ngx-training
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Start the development server**
   ```bash
   npm start
   ```

4. **Open your browser**
   Navigate to `http://localhost:4200`

### Available Scripts

- `npm start` - Start development server
- `npm run build` - Build for production
- `npm test` - Run unit tests
- `npm run serve-build-static` - Serve built application
- `npm run cleanup` - Clean up repository

## 🔧 Development

### Key Components

- **StoreViewComponent** - Main page displaying all license plates
- **CartViewComponent** - Shopping cart management
- **CheckoutViewComponent** - Checkout process
- **CurrencySwitcherComponent** - Currency selection
- **LicensePlateComponent** - Individual plate display

### Services

- **CartService** - Shopping cart operations
- **CurrencyService** - Currency conversion and management
- **LoginService** - User authentication
- **LicensePlateStore** - State management for plates

### Data Models

- **LicensePlate** - Interface for license plate data
- **Currency** - Type for supported currencies

## 🌐 API Endpoints

The application communicates with:

- **License Plate Data**: `https://lp-store-server.vercel.app/data`
- **Exchange Rates**: `https://lp-store-server.vercel.app/rates`
- **Cart Operations**: `http://localhost:8000/cart`
- **Authentication**: `http://localhost:8000/login`

## 🎨 Styling

The application uses Bootstrap 4.6 for responsive design and consistent styling. Custom CSS is minimal and focuses on layout adjustments.

## 🧪 Testing

Run tests with:
```bash
npm test
```

The project includes Jasmine and Karma for unit testing.

## 📖 Learning Resources

For detailed information about:

- **API Reference**: See [API Documentation](API_DOCUMENTATION.md)
- **Component Details**: See [Component Details](COMPONENT_DETAILS.md)
- **Quick Reference**: See [Quick Reference](QUICK_REFERENCE.md)

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## 📄 License

This project is for educational purposes.

## 🆘 Support

For questions or issues, please refer to the comprehensive documentation or create an issue in the repository.
