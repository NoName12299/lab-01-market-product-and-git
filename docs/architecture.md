## Product Choice

Wildberries.ru  
<https://www.wildberries.ru/>  
Wildberries is one of the largest Russian online retailers with an integrated logistics and marketplace platform.

## Main components

![Wildberries Component Diagram](diagrams/out/wildberries/architecture-component/Component%20Diagram.svg)  
![Wildberries Component Diagram code](diagrams/src/wildberries/architecture-component.puml)  
Partner/Seller Gateway: Main API entry point for sellers to manage their products and orders on the marketplace.  
Customer Mobile App: Native mobile application for users to browse, shop, and track purchases.  
Customer Website (SSR): Server-rendered website providing the same shopping experience as the app, optimized for web browsers.  
Catalog & Search Service: Manages product listings, categories, and search functionality, enabling customers to find items quickly.  
Auth & ID Service: Manages user authentication, authorization, and identity verification across all client applications.

## Data flow

![Wildberries Sequence Diagram](diagrams/out/wildberries/architecture-sequence/Sequence%20Diagram.svg)  
![Wildberries Sequence Diagram code](diagrams/src/wildberries/architecture-sequence.puml)  
Preparation (Search & Cart)  
This group covers the user adding items to their cart. The cart data is stored in a fast cache with a time limit, and the user interface is updated to reflect the cart's contents.
The user clicks "Add to Cart" in the mobile app.  
The app sends an RPC call, addToCart, to the Storefront Gateway.  
The gateway forwards the request to the Cart Service.
The Cart Service stores the cart data in Redis cache with a 7‑day expiration.  
Redis confirms the write, and the confirmation flows back through the Cart Service, Gateway, and App.  
Mobile app updates its UI with the new cart total and item counter.

## Deployment

![Wildberries Sequence Diagram](diagrams/out/wildberries/architecture-deployment/Deployment%20Diagram.svg)  
![Wildberries Sequence Diagram code](diagrams/src/wildberries/architecture-deployment.puml)  
The core application (API gateways, business services, data clusters) is hosted on Wildberries' Global Infrastructure, which is depicted as the primary data center (a large cloud-like shape).  
Client-side applications run on end-user devices: Customer Mobile App (on smartphones), Customer Website (in web browsers), WB Partners App (on seller devices), and specialized software on Pickup Point (PVZ) PCs and Warehouse Terminals.  
External systems such as Payment Providers, Third-Party Logistics (3PL), and SMS/Push Providers are located in separate external ecosystems and connect to WB's platform via APIs.  

## Assumptions

I assume the system uses a fast Redis database to store temporary data, such as items in the shopping cart. This helps the site run quickly while the user is shopping.  
I assume there's a single Auth & ID Service that handles user login across all devices (both the app and the website). This ensures password and data security.  

## Open questions

How exactly does the search system decide which products to show first?  
How is my payment information kept safe when I buy something?
