# Atbaq Restaurant — Pre-Order Web App
A customer-facing web application for placing food pre-orders, built for a small
food business. Customers browse the menu, place an order for a future delivery
date/time, and pay in cash on delivery. The owner reviews incoming orders
directly through the database/admin view.
This project is also part of my developer portfolio — see Project rationale
below for the reasoning behind key decisions.
Status
🚧 In active development — v1 (customer-facing MVP) in progress.
Table of contents
Problem statement
Scope
Tech stack
Data model
Screens
Getting started
Project structure
Roadmap
Project rationale
Problem statement
# Atbaq is a home-based/small food business that takes orders a day ahead of
delivery. Previously orders were collected manually (chat, calls). This app
gives customers a self-serve way to browse the menu and submit a pre-order,
with delivery date, time, and address captured up front — reducing back-and-forth
and order entry errors.
Scope
In scope for v1:
Customer-facing menu browsing
Add to cart, review order, checkout
Order capture: customer name, delivery address, delivery date/time, items, quantities
Cash-on-delivery as the only payment method
Owner views orders via database / simple admin form
Explicitly out of scope for v1 (planned for later phases):
Staff-facing kitchen/order management view
Full admin dashboard (menu CRUD via UI, analytics)
Online payment integration
Multi-tenant support (this is single-restaurant only)
Real-time order status updates
Offline support
Tech stack
Layer	Choice	Why
Backend	Java + Spring Boot	Familiar language, mature ecosystem, easy path from basic Java
Templating	Thymeleaf	Server-rendered HTML, avoids adding a separate frontend framework for v1
Data access	Spring Data JPA	Reduces boilerplate SQL, works well with a small relational schema
Database	H2 (dev) → PostgreSQL (production)	H2 for fast local iteration, Postgres for a real deployment
Validation	Spring Validation (Bean Validation)	Server-side validation of order/checkout forms
Data model
Core entities: `Customer`, `Order`, `OrderItem`, `MenuItem`.
A `Customer` places many `Order`s.
An `Order` has many `OrderItem`s (one row per menu item + quantity).
An `OrderItem` references one `MenuItem`.
`Order` carries delivery date, delivery time, delivery address, status, and payment method.
See `/docs/erd.png` (or the diagram in project documentation) for the full
entity-relationship diagram.
Screens
Menu browse — list of menu items by category
Item detail — item info, quantity selector, add to cart
Cart / order review — line items, subtotal, total
Checkout — customer name, delivery address, delivery date/time, payment note (cash on delivery)
Confirmation — order submitted successfully
Wireframes for these screens are in `/docs/wireframes/`.
Getting started
```bash
# clone the repo
git clone https://github.com/rxavierremo/atbaq-restaurant-app.git
cd atbaq-restaurant-app

# run with Maven
./mvnw spring-boot:run

# app available at
http://localhost:8080
```
Requires: Java 17+, Maven (wrapper included).
Project structure
```
src/
  main/
    java/com/[package]/
      controller/     # handles HTTP requests, routes to services
      service/        # business logic (order creation, validation)
      repository/     # Spring Data JPA interfaces
      model/           # entity classes (Customer, Order, OrderItem, MenuItem)
    resources/
      templates/      # Thymeleaf HTML templates
      static/          # CSS, JS, images
docs/
  erd.png
  wireframes/
README.md
```
Roadmap
[x] Define scope, ERD, wireframes
[ ] Set up Spring Boot project, base entities, repositories
[ ] Menu browse + item detail screens
[ ] Cart + checkout flow
[ ] Order confirmation + persistence
[ ] Basic admin order view (read-only)
[ ] Deploy (e.g. Render/Railway) with production DB
[ ] Staff-facing order management (phase 2)
[ ] Online payment integration (phase 2)
Project rationale
Notes for anyone reviewing this as a portfolio project:
Why pre-order instead of real-time ordering: the business currently
fulfills orders the day after they're placed, so real-time kitchen/order
sync wasn't needed for v1 — this kept the initial scope focused and avoided
premature complexity (websockets, live status).
Why Spring Boot + Thymeleaf over a separate frontend: prioritized
shipping a working v1 with a single, well-documented stack rather than
splitting effort across a Java backend and a JS frontend framework.
Why cash-only first: online payment integration adds real complexity
(PCI concerns, provider APIs, error handling for failed payments) that
wasn't necessary to validate the ordering flow itself. It's scoped as a
clearly separated v2 feature rather than bolted on.
