# Complete Angular Guide for Beginners

A comprehensive guide covering all essential Angular concepts with technical explanations, easy-to-understand metaphors, code examples, and practical usage scenarios.

---

## Table of Contents
1. [What is Angular?](#what-is-angular)
2. [Components](#components)
3. [Modules](#modules)
4. [Templates & Data Binding](#templates--data-binding)
5. [Directives](#directives)
6. [Services & Dependency Injection](#services--dependency-injection)
7. [Routing](#routing)
8. [Observables & RxJS](#observables--rxjs)
9. [Forms](#forms)
10. [Pipes](#pipes)
11. [Lifecycle Hooks](#lifecycle-hooks)
12. [HTTP Client](#http-client)
13. [Links](#links)

---

## What is Angular?

**Technical Explanation:**
Angular is a TypeScript-based, open-source web application framework developed by Google. It follows the Model-View-Controller (MVC) architectural pattern and provides a complete solution for building single-page applications (SPAs) with features like two-way data binding, dependency injection, routing, and more.

**Metaphor:**
Think of Angular as a complete construction kit for building a house. Instead of gathering individual tools and materials from different places, Angular gives you everything you need in one box: the blueprint (structure), tools (utilities), and instructions (documentation) to build a modern web application.

**When & Why to Use:**
- Building large-scale enterprise applications
- Need for structured, maintainable code
- Requiring built-in features like routing, forms, HTTP
- Teams that benefit from opinionated frameworks
- Projects requiring long-term maintenance

---

## Components

**Technical Explanation:**
Components are the fundamental building blocks of Angular applications. Each component consists of three parts: a TypeScript class (logic), an HTML template (view), and CSS styles (presentation). Components control a portion of the screen called a view and can be nested, reused, and composed together.

**Metaphor:**
Think of components as LEGO blocks. Each block (component) is self-contained with its own shape, color, and purpose. You can snap them together to build complex structures (your application). A button component is like a small LEGO piece, while a navigation bar is a larger assembly of multiple pieces.

**Code Example:**
```typescript
// hero.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-hero',
  templateUrl: './hero.component.html',
  styleUrls: ['./hero.component.css']
})
export class HeroComponent {
  heroName: string = 'Spider-Man';
  heroLevel: number = 10;
  
  powerUp(): void {
    this.heroLevel++;
    console.log(`${this.heroName} leveled up to ${this.heroLevel}!`);
  }
}
```

```html
<!-- hero.component.html -->
<div class="hero-card">
  <h2>{{ heroName }}</h2>
  <p>Level: {{ heroLevel }}</p>
  <button (click)="powerUp()">Power Up!</button>
</div>
```

**When & Why to Use:**
- Breaking down UI into manageable, reusable pieces
- Encapsulating related functionality
- Creating reusable widgets (buttons, cards, modals)
- Organizing code by feature or domain
- When you need to repeat UI patterns across the app

---

## Modules

**Technical Explanation:**
NgModules are containers that group related components, directives, pipes, and services. They organize an application into cohesive blocks of functionality. Every Angular app has at least one module, the root module (AppModule), which bootstraps the application. Modules can import and export functionality to share between different parts of the app.

**Metaphor:**
Modules are like departments in a company. The Marketing Department (module) has its own employees (components), tools (services), and processes (directives). Departments can share resources with each other—HR might share the payroll service with all departments, just like modules can share services.

**Code Example:**
```typescript
// app.module.ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { FormsModule } from '@angular/forms';
import { HttpClientModule } from '@angular/common/http';

import { AppComponent } from './app.component';
import { HeroComponent } from './hero/hero.component';
import { VillainComponent } from './villain/villain.component';
import { BattleService } from './services/battle.service';

@NgModule({
  declarations: [
    AppComponent,      // Components, directives, pipes
    HeroComponent,
    VillainComponent
  ],
  imports: [
    BrowserModule,     // Other modules this module needs
    FormsModule,
    HttpClientModule
  ],
  providers: [
    BattleService      // Services available for dependency injection
  ],
  bootstrap: [AppComponent]  // Root component to bootstrap
})
export class AppModule { }
```

**Feature Module Example:**
```typescript
// heroes.module.ts
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';

import { HeroListComponent } from './hero-list/hero-list.component';
import { HeroDetailComponent } from './hero-detail/hero-detail.component';
import { HeroService } from './hero.service';

@NgModule({
  declarations: [
    HeroListComponent,
    HeroDetailComponent
  ],
  imports: [
    CommonModule  // Provides common directives like ngIf, ngFor
  ],
  providers: [HeroService],
  exports: [HeroListComponent]  // Make available to other modules
})
export class HeroesModule { }
```

**When & Why to Use:**
- Organizing application by feature (user module, admin module)
- Lazy loading parts of the app to improve performance
- Sharing common functionality across the app
- Encapsulating third-party libraries
- Separating concerns in large applications

---

## Templates & Data Binding

**Technical Explanation:**
Templates define the component's view using HTML enhanced with Angular's template syntax. Data binding is the mechanism that coordinates communication between the component class and the template. Angular supports four types: interpolation `{{ }}`, property binding `[property]`, event binding `(event)`, and two-way binding `[(ngModel)]`.

**Metaphor:**
Templates and data binding are like a live TV broadcast. The component is the studio (where data lives), and the template is the TV screen (what users see). Interpolation is like showing what's happening in the studio on screen. Event binding is like viewers calling in to affect the show. Two-way binding is like a live interactive broadcast where the studio and viewers communicate simultaneously.

**Code Examples:**

```typescript
// data-binding.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-data-binding',
  template: `
    <!-- Interpolation: Component to View -->
    <h1>{{ title }}</h1>
    <p>Current count: {{ count }}</p>
    
    <!-- Property Binding: Component to View -->
    <img [src]="imageUrl" [alt]="imageDescription">
    <button [disabled]="isDisabled">Click Me</button>
    
    <!-- Event Binding: View to Component -->
    <button (click)="increment()">Increment</button>
    <input (keyup)="onKey($event)">
    
    <!-- Two-Way Binding: Bidirectional -->
    <input [(ngModel)]="username">
    <p>Hello, {{ username }}!</p>
    
    <!-- Class & Style Binding -->
    <div [class.active]="isActive">Active Div</div>
    <p [style.color]="textColor">Colored Text</p>
  `
})
export class DataBindingComponent {
  title = 'Data Binding Demo';
  count = 0;
  imageUrl = 'assets/logo.png';
  imageDescription = 'Company Logo';
  isDisabled = false;
  username = '';
  isActive = true;
  textColor = 'blue';
  
  increment(): void {
    this.count++;
  }
  
  onKey(event: KeyboardEvent): void {
    console.log('Key pressed:', (event.target as HTMLInputElement).value);
  }
}
```

**Template Reference Variables:**
```html
<input #phoneInput type="text">
<button (click)="callNumber(phoneInput.value)">Call</button>

<!-- Reference to components -->
<app-hero #heroComponent></app-hero>
<button (click)="heroComponent.powerUp()">Power Up Hero</button>
```

**When & Why to Use:**
- **Interpolation**: Displaying dynamic text content
- **Property Binding**: Setting element properties, attributes, and component inputs
- **Event Binding**: Responding to user actions (clicks, keystrokes, mouse movements)
- **Two-Way Binding**: Forms where you need instant sync between view and model
- **Template References**: Accessing DOM elements or child components directly

---

## Directives

**Technical Explanation:**
Directives are classes that add behavior to elements in Angular applications. There are three types: Components (directives with templates), Structural directives (change DOM layout by adding/removing elements - prefixed with `*`), and Attribute directives (change appearance or behavior of elements).

**Metaphor:**
Directives are like instructions you give to your assistant. Structural directives (ngIf, ngFor) are like saying "If it's raining, get an umbrella" or "For each guest, set up a chair" - they control whether things exist. Attribute directives (ngClass, ngStyle) are like "Paint this wall blue" or "Make this text bold" - they modify existing things.

**Code Examples:**

### Structural Directives

```typescript
// directives-demo.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-directives-demo',
  template: `
    <!-- *ngIf: Conditional Rendering -->
    <div *ngIf="isLoggedIn">
      Welcome back, {{ username }}!
    </div>
    <div *ngIf="!isLoggedIn">
      Please log in.
    </div>
    
    <!-- *ngIf with else -->
    <div *ngIf="hasData; else noData">
      <p>Data loaded successfully!</p>
    </div>
    <ng-template #noData>
      <p>No data available.</p>
    </ng-template>
    
    <!-- *ngFor: Loop through arrays -->
    <ul>
      <li *ngFor="let hero of heroes; let i = index; let first = first">
        {{ i + 1 }}. {{ hero.name }}
        <span *ngIf="first">(Leader)</span>
      </li>
    </ul>
    
    <!-- *ngSwitch: Multiple conditions -->
    <div [ngSwitch]="userRole">
      <p *ngSwitchCase="'admin'">Admin Dashboard</p>
      <p *ngSwitchCase="'user'">User Dashboard</p>
      <p *ngSwitchCase="'guest'">Guest View</p>
      <p *ngSwitchDefault>Unknown Role</p>
    </div>
  `
})
export class DirectivesDemoComponent {
  isLoggedIn = true;
  username = 'Spider-Man';
  hasData = false;
  userRole = 'admin';
  
  heroes = [
    { name: 'Iron Man' },
    { name: 'Captain America' },
    { name: 'Thor' }
  ];
}
```

### Attribute Directives

```typescript
@Component({
  selector: 'app-attribute-directives',
  template: `
    <!-- ngClass: Dynamic classes -->
    <div [ngClass]="{'active': isActive, 'disabled': isDisabled, 'highlight': true}">
      Multiple classes
    </div>
    
    <div [ngClass]="currentClasses">Dynamic class object</div>
    
    <!-- ngStyle: Dynamic styles -->
    <p [ngStyle]="{'color': textColor, 'font-size': fontSize + 'px'}">
      Styled text
    </p>
    
    <div [ngStyle]="currentStyles">Dynamic style object</div>
    
    <!-- ngModel: Two-way binding (needs FormsModule) -->
    <input [(ngModel)]="heroName">
    <p>Hero: {{ heroName }}</p>
  `
})
export class AttributeDirectivesComponent {
  isActive = true;
  isDisabled = false;
  textColor = 'red';
  fontSize = 16;
  heroName = 'Batman';
  
  currentClasses = {
    'active': this.isActive,
    'large': this.fontSize > 14
  };
  
  currentStyles = {
    'font-weight': 'bold',
    'background-color': '#f0f0f0'
  };
}
```

### Custom Directive

```typescript
// highlight.directive.ts
import { Directive, ElementRef, HostListener, Input } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  @Input() appHighlight = 'yellow';
  @Input() defaultColor = 'transparent';
  
  constructor(private el: ElementRef) {}
  
  @HostListener('mouseenter') onMouseEnter() {
    this.highlight(this.appHighlight);
  }
  
  @HostListener('mouseleave') onMouseLeave() {
    this.highlight(this.defaultColor);
  }
  
  private highlight(color: string) {
    this.el.nativeElement.style.backgroundColor = color;
  }
}
```

```html
<!-- Using custom directive -->
<p appHighlight="lightblue" defaultColor="white">
  Hover over me!
</p>
```

**When & Why to Use:**
- **ngIf**: Show/hide content based on conditions (loading states, permissions)
- **ngFor**: Display lists (products, users, menu items)
- **ngSwitch**: Multiple conditional views (dashboards, role-based content)
- **ngClass**: Dynamic styling based on state (active tabs, validation states)
- **ngStyle**: Inline dynamic styles (themes, user preferences)
- **Custom Directives**: Reusable behaviors (tooltips, drag-and-drop, auto-focus)

---

## Services & Dependency Injection

**Technical Explanation:**
Services are singleton objects that encapsulate business logic, data access, or any functionality that doesn't belong in components. Dependency Injection (DI) is a design pattern where a class receives its dependencies from external sources rather than creating them itself. Angular's DI framework provides dependencies to classes upon instantiation, promoting loose coupling and testability.

**Metaphor:**
A service is like a specialized craftsman in a medieval village. Instead of every villager (component) learning to blacksmith, bake, and farm, they hire specialists (services). The village coordinator (DI) introduces villagers to the specialists they need. When you need bread, the coordinator connects you with the baker service. You don't need to know how to bake or even where the baker lives—you just get your bread.

**Code Examples:**

### Creating a Service

```typescript
// hero.service.ts
import { Injectable } from '@angular/core';
import { Observable, of } from 'rxjs';

export interface Hero {
  id: number;
  name: string;
  power: string;
}

@Injectable({
  providedIn: 'root'  // Service available app-wide (singleton)
})
export class HeroService {
  private heroes: Hero[] = [
    { id: 1, name: 'Iron Man', power: 'Technology' },
    { id: 2, name: 'Thor', power: 'Lightning' },
    { id: 3, name: 'Hulk', power: 'Strength' }
  ];
  
  getHeroes(): Observable<Hero[]> {
    // Simulate async operation
    return of(this.heroes);
  }
  
  getHero(id: number): Observable<Hero | undefined> {
    const hero = this.heroes.find(h => h.id === id);
    return of(hero);
  }
  
  addHero(hero: Hero): void {
    this.heroes.push(hero);
  }
  
  deleteHero(id: number): void {
    this.heroes = this.heroes.filter(h => h.id !== id);
  }
  
  updateHero(updatedHero: Hero): void {
    const index = this.heroes.findIndex(h => h.id === updatedHero.id);
    if (index !== -1) {
      this.heroes[index] = updatedHero;
    }
  }
}
```

### Using Dependency Injection

```typescript
// hero-list.component.ts
import { Component, OnInit } from '@angular/core';
import { HeroService, Hero } from '../services/hero.service';
import { LoggerService } from '../services/logger.service';

@Component({
  selector: 'app-hero-list',
  template: `
    <h2>Heroes</h2>
    <ul>
      <li *ngFor="let hero of heroes">
        {{ hero.name }} - {{ hero.power }}
        <button (click)="remove(hero.id)">Delete</button>
      </li>
    </ul>
    <button (click)="addNewHero()">Add Hero</button>
  `
})
export class HeroListComponent implements OnInit {
  heroes: Hero[] = [];
  
  // Services injected via constructor
  constructor(
    private heroService: HeroService,
    private logger: LoggerService
  ) {}
  
  ngOnInit(): void {
    this.loadHeroes();
  }
  
  loadHeroes(): void {
    this.heroService.getHeroes().subscribe(heroes => {
      this.heroes = heroes;
      this.logger.log('Heroes loaded successfully');
    });
  }
  
  remove(id: number): void {
    this.heroService.deleteHero(id);
    this.loadHeroes();
    this.logger.log(`Hero ${id} deleted`);
  }
  
  addNewHero(): void {
    const newHero: Hero = {
      id: Date.now(),
      name: 'Black Widow',
      power: 'Combat Skills'
    };
    this.heroService.addHero(newHero);
    this.loadHeroes();
  }
}
```

### Service with Dependencies

```typescript
// logger.service.ts
@Injectable({
  providedIn: 'root'
})
export class LoggerService {
  log(message: string): void {
    console.log(`[LOG] ${new Date().toISOString()}: ${message}`);
  }
  
  error(message: string): void {
    console.error(`[ERROR] ${new Date().toISOString()}: ${message}`);
  }
}
```

### Different Provider Scopes

```typescript
// Module-level provider (shared within module)
@NgModule({
  providers: [HeroService]  // New instance per module
})
export class HeroesModule { }

// Component-level provider (new instance per component)
@Component({
  selector: 'app-hero',
  providers: [HeroService]  // New instance for this component and children
})
export class HeroComponent { }

// Root-level provider (app-wide singleton) - RECOMMENDED
@Injectable({
  providedIn: 'root'
})
export class HeroService { }
```

**When & Why to Use:**
- **Services**: Sharing data/logic between components, API calls, business logic
- **Root-provided services**: App-wide functionality (authentication, logging, configuration)
- **Module-provided services**: Feature-specific services
- **Component-provided services**: Component-specific state that shouldn't be shared
- **DI Benefits**: Easier testing (mock dependencies), cleaner code, loose coupling

---

## Routing

**Technical Explanation:**
Angular Router enables navigation between different views/components in a single-page application. It interprets browser URLs as instructions to navigate to a client-generated view, manages route parameters, guards routes with authentication logic, and supports lazy loading of feature modules. The router uses HTML5 pushState for clean URLs.

**Metaphor:**
The router is like a GPS navigation system for your app. When users type an address (URL) or click a link (navigation), the GPS (router) figures out which view (destination) to show and how to get there. Route guards are like checkpoints that verify you have permission to enter certain areas. Route parameters are like specific apartment numbers in a building address.

**Code Examples:**

### Basic Router Setup

```typescript
// app-routing.module.ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';

import { HomeComponent } from './home/home.component';
import { HeroListComponent } from './heroes/hero-list.component';
import { HeroDetailComponent } from './heroes/hero-detail.component';
import { PageNotFoundComponent } from './page-not-found/page-not-found.component';
import { AboutComponent } from './about/about.component';

const routes: Routes = [
  { path: '', redirectTo: '/home', pathMatch: 'full' },
  { path: 'home', component: HomeComponent },
  { path: 'heroes', component: HeroListComponent },
  { path: 'heroes/:id', component: HeroDetailComponent },  // Route parameter
  { path: 'about', component: AboutComponent },
  { path: '**', component: PageNotFoundComponent }  // Wildcard route (404)
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

### App Component with Router Outlet

```typescript
// app.component.ts
@Component({
  selector: 'app-root',
  template: `
    <nav>
      <a routerLink="/home" routerLinkActive="active">Home</a>
      <a routerLink="/heroes" routerLinkActive="active">Heroes</a>
      <a routerLink="/about" routerLinkActive="active">About</a>
    </nav>
    
    <!-- Routed components display here -->
    <router-outlet></router-outlet>
  `,
  styles: [`
    nav { background: #333; padding: 10px; }
    a { color: white; margin: 0 10px; text-decoration: none; }
    .active { font-weight: bold; border-bottom: 2px solid white; }
  `]
})
export class AppComponent { }
```

### Navigating Programmatically

```typescript
// hero-list.component.ts
import { Component } from '@angular/core';
import { Router } from '@angular/router';

@Component({
  selector: 'app-hero-list',
  template: `
    <div *ngFor="let hero of heroes">
      <button (click)="viewHero(hero.id)">View {{ hero.name }}</button>
    </div>
  `
})
export class HeroListComponent {
  heroes = [
    { id: 1, name: 'Iron Man' },
    { id: 2, name: 'Thor' }
  ];
  
  constructor(private router: Router) {}
  
  viewHero(id: number): void {
    // Navigate programmatically
    this.router.navigate(['/heroes', id]);
  }
}
```

### Accessing Route Parameters

```typescript
// hero-detail.component.ts
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute, ParamMap } from '@angular/router';
import { switchMap } from 'rxjs/operators';
import { HeroService } from '../services/hero.service';

@Component({
  selector: 'app-hero-detail',
  template: `
    <div *ngIf="hero">
      <h2>{{ hero.name }}</h2>
      <p>Power: {{ hero.power }}</p>
      <button (click)="goBack()">Go Back</button>
    </div>
  `
})
export class HeroDetailComponent implements OnInit {
  hero: any;
  
  constructor(
    private route: ActivatedRoute,
    private heroService: HeroService,
    private router: Router
  ) {}
  
  ngOnInit(): void {
    // Method 1: Snapshot (for static parameters)
    const id = Number(this.route.snapshot.paramMap.get('id'));
    
    // Method 2: Observable (for dynamic parameters)
    this.route.paramMap.pipe(
      switchMap((params: ParamMap) =>
        this.heroService.getHero(Number(params.get('id')))
      )
    ).subscribe(hero => this.hero = hero);
  }
  
  goBack(): void {
    this.router.navigate(['/heroes']);
  }
}
```

### Route Guards

```typescript
// auth.guard.ts
import { Injectable } from '@angular/core';
import { CanActivate, Router, ActivatedRouteSnapshot, RouterStateSnapshot } from '@angular/router';
import { AuthService } from './auth.service';

@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanActivate {
  constructor(
    private authService: AuthService,
    private router: Router
  ) {}
  
  canActivate(
    route: ActivatedRouteSnapshot,
    state: RouterStateSnapshot
  ): boolean {
    if (this.authService.isLoggedIn()) {
      return true;
    }
    
    // Redirect to login
    this.router.navigate(['/login'], {
      queryParams: { returnUrl: state.url }
    });
    return false;
  }
}

// Using the guard
const routes: Routes = [
  {
    path: 'admin',
    component: AdminComponent,
    canActivate: [AuthGuard]  // Protected route
  }
];
```

### Lazy Loading

```typescript
// app-routing.module.ts
const routes: Routes = [
  {
    path: 'admin',
    loadChildren: () => import('./admin/admin.module').then(m => m.AdminModule),
    canActivate: [AuthGuard]
  }
];

// admin-routing.module.ts (in admin folder)
const routes: Routes = [
  { path: '', component: AdminDashboardComponent },
  { path: 'users', component: UserManagementComponent }
];

@NgModule({
  imports: [RouterModule.forChild(routes)],  // forChild for feature modules
  exports: [RouterModule]
})
export class AdminRoutingModule { }
```

### Query Parameters

```typescript
// Navigate with query params
this.router.navigate(['/heroes'], {
  queryParams: { filter: 'active', sort: 'name' }
});
// URL: /heroes?filter=active&sort=name

// Read query params
this.route.queryParams.subscribe(params => {
  const filter = params['filter'];
  const sort = params['sort'];
});
```

**When & Why to Use:**
- **Basic routing**: Multi-page feel in SPA
- **Route parameters**: Detail pages (product/:id, user/:username)
- **Query parameters**: Filtering, pagination, search terms
- **Guards**: Authentication, authorization, unsaved changes warnings
- **Lazy loading**: Large apps to reduce initial bundle size
- **Nested routes**: Complex hierarchical navigation

---

## Observables & RxJS

**Technical Explanation:**
Observables are lazy Push collections of multiple values over time, part of the RxJS library. Unlike Promises (which resolve once), Observables can emit multiple values. They're fundamental to Angular's reactive programming approach, used extensively in HTTP, routing, forms, and event handling. RxJS provides operators to transform, combine, and manage async data streams.

**Metaphor:**
An Observable is like a newspaper subscription. When you subscribe, you get multiple issues (values) delivered over time. You can cancel anytime (unsubscribe). Operators are like filters—you might only want sports news (filter), or want summaries instead of full articles (map). A Promise is like ordering a single book—once delivered, that's it.

**Code Examples:**

### Basic Observable

```typescript
import { Component, OnInit, OnDestroy } from '@angular/core';
import { Observable, Subscription, interval, of, from } from 'rxjs';
import { map, filter, take } from 'rxjs/operators';

@Component({
  selector: 'app-observable-demo',
  template: `
    <h2>Current Count: {{ count }}</h2>
    <p>Even numbers: {{ evenNumbers | json }}</p>
  `
})
export class ObservableDemoComponent implements OnInit, OnDestroy {
  count = 0;
  evenNumbers: number[] = [];
  private subscription!: Subscription;
  
  ngOnInit(): void {
    // Create an observable that emits every second
    const numbers$ = interval(1000);
    
    // Subscribe to the observable
    this.subscription = numbers$.pipe(
      take(10),  // Only take first 10 values
      map(n => n + 1),  // Transform values
      filter(n => n % 2 === 0)  // Only even numbers
    ).subscribe({
      next: (value) => {
        this.count = value;
        this.evenNumbers.push(value);
      },
      error: (err) => console.error('Error:', err),
      complete: () => console.log('Observable completed')
    });
  }
  
  ngOnDestroy(): void {
    // Always unsubscribe to prevent memory leaks
    if (this.subscription) {
      this.subscription.unsubscribe();
    }
  }
}
```

### Common RxJS Operators

```typescript
import { Component, OnInit } from '@angular/core';
import { of, from, interval, Subject } from 'rxjs';
import { 
  map, filter, debounceTime, distinctUntilChanged,
  switchMap, mergeMap, catchError, tap, take
} from 'rxjs/operators';

@Component({
  selector: 'app-rxjs-operators'
})
export class RxjsOperatorsComponent implements OnInit {
  
  ngOnInit(): void {
    this.demonstrateOperators();
  }
  
  demonstrateOperators(): void {
    // MAP: Transform values
    of(1, 2, 3, 4, 5).pipe(
      map(x => x * 10)
    ).subscribe(val => console.log('Map:', val));
    // Output: 10, 20, 30, 40, 50
    
    // FILTER: Select specific values
    of(1, 2, 3, 4, 5).pipe(
      filter(x => x % 2 === 0)
    ).subscribe(val => console.log('Filter:', val));
    // Output: 2, 4
    
    // TAP: Side effects (debugging, logging)
    of(1, 2, 3).pipe(
      tap(x => console.log('Before map:', x)),
      map(x => x * 2),
      tap(x => console.log('After map:', x))
    ).subscribe();
    
    // DEBOUNCE: Wait for pause in emissions
    const searchInput$ = new Subject<string>();
    searchInput$.pipe(
      debounceTime(300),  // Wait 300ms after last keystroke
      distinctUntilChanged()  // Ignore if same as previous
    ).subscribe(term => console.log('Search for:', term));
    
    // SWITCHMAP: Cancel previous, switch to new observable
    // Great for search or API calls
    searchInput$.pipe(
      debounceTime(300),
      switchMap(term => this.searchAPI(term))
    ).subscribe(results => console.log('Results:', results));
    
    // CATCHERROR: Handle errors gracefully
    this.fetchData().pipe(
      catchError(err => {
        console.error('Error caught:', err);
        return of({ error: true, message: 'Failed to load data' });
      })
    ).subscribe(data => console.log('Data:', data));
    
    // TAKE: Only take first N emissions
    interval(1000).pipe(
      take(5)
    ).subscribe(val => console.log('Take 5:', val));
    // Emits 0,1,2,3,4 then completes
  }
  
  searchAPI(term: string): Observable<any> {
    // Simulated API call
    return of({ results: [`Result for ${term}`] });
  }
  
  fetchData(): Observable<any> {
    return of({ data: 'some data' });
  }
}
```

### Subject - Multicast Observable

```typescript
import { Subject, BehaviorSubject, ReplaySubject } from 'rxjs';

// SUBJECT: Basic multicast
const subject = new Subject<number>();

subject.subscribe(val => console.log('Observer 1:', val));
subject.subscribe(val => console.log('Observer 2:', val));

subject.next(1);  // Both observers receive 1
subject.next(2);  // Both observers receive 2

// BEHAVIORSUBJECT: Stores current value, emits immediately on subscribe
const currentUser$ = new BehaviorSubject<string>('Guest');

currentUser$.subscribe(user => console.log('User 1:', user));  // Immediately logs 'Guest'

currentUser$.next('John');  // Emits 'John' to all subscribers

currentUser$.subscribe(user => console.log('User 2:', user));  // Immediately logs 'John'

// REPLAYSUBJECT: Replays last N values to new subscribers
const messages$ = new ReplaySubject<string>(2);  // Replay last 2 values

messages$.next('Message 1');
messages$.next('Message 2');
messages$.next('Message 3');

messages$.subscribe(msg => console.log('Late subscriber:', msg));
// Logs: 'Message 2', 'Message 3'
```

### Async Pipe (Auto-subscribing)

```typescript
@Component({
  selector: 'app-async-pipe-demo',
  template: `
    <!-- Async pipe automatically subscribes and unsubscribes -->
    <div *ngIf="heroes$ | async as heroes; else loading">
      <h3>Heroes</h3>
      <ul>
        <li *ngFor="let hero of heroes">{{ hero.name }}</li>
      </ul>
    </div>
    
    <ng-template #loading>
      <p>Loading heroes...</p>
    </ng-template>
    
    <p>Current time: {{ time$ | async | date:'medium' }}</p>
  `
})
export class AsyncPipeDemoComponent implements OnInit {
  heroes$!: Observable<Hero[]>;
  time$!: Observable<Date>;
  
  constructor(private heroService: HeroService) {}
  
  ngOnInit(): void {
    this.heroes$ = this.heroService.getHeroes();
    this.time$ = interval(1000).pipe(
      map(() => new Date())
    );
  }
  // No need to unsubscribe - async pipe handles it!
}
```

**When & Why to Use:**
- **Observables**: Any async operation (HTTP, events, timers)
- **Operators**: Transforming, filtering, combining data streams
- **Subject**: Event bus, sharing data between unrelated components
- **BehaviorSubject**: State management, current value storage (user session, theme)
- **debounceTime**: Search inputs, window resize events
- **switchMap**: API calls that can be cancelled (search, autocomplete)
- **Async pipe**: Template subscriptions (automatic cleanup)

---

## Forms

**Technical Explanation:**
Angular provides two approaches to handling forms: Template-driven forms (Angular creates form model automatically based on template directives) and Reactive forms (you explicitly create form model in component class). Reactive forms are more scalable, testable, and provide synchronous access to data model. Both support validation, change tracking, and error handling.

**Metaphor:**
Template-driven forms are like ordering from a menu—you tell the waiter (template) what you want, and the kitchen (Angular) figures out how to make it. Reactive forms are like cooking yourself—you have complete control over ingredients (form controls), recipe (form model), and timing (value changes). Template-driven is simpler for basic meals; reactive is better for complex dishes that need precise control.

### Template-Driven Forms

```typescript
// app.module.ts - Import FormsModule
import { FormsModule } from '@angular/forms';

@NgModule({
  imports: [FormsModule]
})
export class AppModule { }

// template-driven.component.ts
import { Component } from '@angular/core';

interface UserModel {
  name: string;
  email: string;
  age: number;
  country: string;
  terms: boolean;
}

@Component({
  selector: 'app-template-driven',
  template: `
    <form #userForm="ngForm" (ngSubmit)="onSubmit(userForm)">
      
      <!-- Text Input with Validation -->
      <div>
        <label>Name:</label>
        <input 
          type="text" 
          name="name"
          [(ngModel)]="model.name"
          #name="ngModel"
          required
          minlength="3">
        
        <div *ngIf="name.invalid && name.touched" class="error">
          <p *ngIf="name.errors?.['required']">Name is required</p>
          <p *ngIf="name.errors?.['minlength']">Name must be at least 3 characters</p>
        </div>
      </div>
      
      <!-- Email with Pattern Validation -->
      <div>
        <label>Email:</label>
        <input 
          type="email" 
          name="email"
          [(ngModel)]="model.email"
          #email="ngModel"
          required
          pattern="[a-z0-9._%+-]+@[a-z0-9.-]+\.[a-z]{2,}$">
        
        <div *ngIf="email.invalid && email.touched" class="error">
          <p *ngIf="email.errors?.['required']">Email is required</p>
          <p *ngIf="email.errors?.['pattern']">Invalid email format</p>
        </div>
      </div>
      
      <!-- Number Input -->
      <div>
        <label>Age:</label>
        <input 
          type="number" 
          name="age"
          [(ngModel)]="model.age"
          #age="ngModel"
          required
          min="18"
          max="100">
        
        <div *ngIf="age.invalid && age.touched" class="error">
          <p *ngIf="age.errors?.['min']">Must be at least 18</p>
          <p *ngIf="age.errors?.['max']">Must be under 100</p>
        </div>
      </div>
      
      <!-- Select Dropdown -->
      <div>
        <label>Country:</label>
        <select name="country" [(ngModel)]="model.country" required>
          <option value="">Choose...</option>
          <option value="usa">USA</option>
          <option value="uk">UK</option>
          <option value="canada">Canada</option>
        </select>
      </div>
      
      <!-- Checkbox -->
      <div>
        <label>
          <input 
            type="checkbox" 
            name="terms"
            [(ngModel)]="model.terms"
            required>
          I agree to terms
        </label>
      </div>
      
      <!-- Submit Button -->
      <button type="submit" [disabled]="userForm.invalid">Submit</button>
      
      <!-- Form Debug Info -->
      <pre>Form Valid: {{ userForm.valid }}</pre>
      <pre>Form Value: {{ userForm.value | json }}</pre>
    </form>
  `,
  styles: [`
    .error { color: red; font-size: 0.9em; }
    input.ng-invalid.ng-touched { border-color: red; }
    input.ng-valid.ng-touched { border-color: green; }
  `]
})
export class TemplateDrivenComponent {
  model: UserModel = {
    name: '',
    email: '',
    age: 0,
    country: '',
    terms: false
  };
  
  onSubmit(form: any): void {
    if (form.valid) {
      console.log('Form submitted:', this.model);
      form.reset();
    }
  }
}
```

### Reactive Forms

```typescript
// app.module.ts - Import ReactiveFormsModule
import { ReactiveFormsModule } from '@angular/forms';

@NgModule({
  imports: [ReactiveFormsModule]
})
export class AppModule { }

// reactive-form.component.ts
import { Component, OnInit } from '@angular/core';
import { 
  FormBuilder, 
  FormGroup, 
  FormControl, 
  Validators,
  AbstractControl,
  ValidationErrors
} from '@angular/forms';

@Component({
  selector: 'app-reactive-form',
  template: `
    <form [formGroup]="registrationForm" (ngSubmit)="onSubmit()">
      
      <!-- Username -->
      <div>
        <label>Username:</label>
        <input type="text" formControlName="username">
        <div *ngIf="username?.invalid && username?.touched" class="error">
          <p *ngIf="username?.errors?.['required']">Username required</p>
          <p *ngIf="username?.errors?.['minlength']">Min 3 characters</p>
          <p *ngIf="username?.errors?.['usernameTaken']">Username taken</p>
        </div>
      </div>
      
      <!-- Email -->
      <div>
        <label>Email:</label>
        <input type="email" formControlName="email">
        <div *ngIf="email?.invalid && email?.touched" class="error">
          <p *ngIf="email?.errors?.['required']">Email required</p>
          <p *ngIf="email?.errors?.['email']">Invalid email</p>
        </div>
      </div>
      
      <!-- Password Group with Custom Validator -->
      <div formGroupName="passwordGroup">
        <div>
          <label>Password:</label>
          <input type="password" formControlName="password">
          <div *ngIf="password?.invalid && password?.touched" class="error">
            <p *ngIf="password?.errors?.['required']">Password required</p>
            <p *ngIf="password?.errors?.['minlength']">Min 8 characters</p>
          </div>
        </div>
        
        <div>
          <label>Confirm Password:</label>
          <input type="password" formControlName="confirmPassword">
        </div>
        
        <div *ngIf="passwordGroup?.errors?.['passwordMismatch'] && 
                    passwordGroup?.touched" class="error">
          <p>Passwords do not match</p>
        </div>
      </div>
      
      <!-- Dynamic Form Array -->
      <div>
        <h3>Phone Numbers</h3>
        <div formArrayName="phones">
          <div *ngFor="let phone of phones.controls; let i = index">
            <input [formControlName]="i" placeholder="Phone number">
            <button type="button" (click)="removePhone(i)">Remove</button>
          </div>
        </div>
        <button type="button" (click)="addPhone()">Add Phone</button>
      </div>
      
      <!-- Submit -->
      <button type="submit" [disabled]="registrationForm.invalid">
        Register
      </button>
      
      <!-- Debug -->
      <pre>Form Status: {{ registrationForm.status }}</pre>
      <pre>Form Value: {{ registrationForm.value | json }}</pre>
    </form>
  `
})
export class ReactiveFormComponent implements OnInit {
  registrationForm!: FormGroup;
  
  constructor(private fb: FormBuilder) {}
  
  ngOnInit(): void {
    this.registrationForm = this.fb.group({
      username: ['', [
        Validators.required,
        Validators.minLength(3),
        this.usernameValidator  // Custom validator
      ]],
      email: ['', [Validators.required, Validators.email]],
      passwordGroup: this.fb.group({
        password: ['', [Validators.required, Validators.minLength(8)]],
        confirmPassword: ['', Validators.required]
      }, { validators: this.passwordMatchValidator }),  // Group validator
      phones: this.fb.array([
        this.fb.control('', Validators.pattern(/^\d{10}$/))
      ])
    });
    
    // React to value changes
    this.registrationForm.get('username')?.valueChanges.subscribe(value => {
      console.log('Username changed:', value);
    });
  }
  
  // Custom validator
  usernameValidator(control: AbstractControl): ValidationErrors | null {
    const forbidden = ['admin', 'root', 'superuser'];
    return forbidden.includes(control.value?.toLowerCase()) 
      ? { usernameTaken: true } 
      : null;
  }
  
  // Group validator
  passwordMatchValidator(group: AbstractControl): ValidationErrors | null {
    const password = group.get('password')?.value;
    const confirmPassword = group.get('confirmPassword')?.value;
    return password === confirmPassword ? null : { passwordMismatch: true };
  }
  
  // Getters for easier access
  get username() { return this.registrationForm.get('username'); }
  get email() { return this.registrationForm.get('email'); }
  get passwordGroup() { return this.registrationForm.get('passwordGroup'); }
  get password() { return this.registrationForm.get('passwordGroup.password'); }
  get phones() { return this.registrationForm.get('phones') as FormArray; }
  
  addPhone(): void {
    this.phones.push(this.fb.control('', Validators.pattern(/^\d{10}$/)));
  }
  
  removePhone(index: number): void {
    this.phones.removeAt(index);
  }
  
  onSubmit(): void {
    if (this.registrationForm.valid) {
      console.log('Form submitted:', this.registrationForm.value);
      this.registrationForm.reset();
    }
  }
}
```

### Form Control States

```typescript
// A form control has several states:
// - touched/untouched: User has focused/blurred the field
// - dirty/pristine: Value has/hasn't been changed
// - valid/invalid: Passes/fails validation
// - pending: Async validation in progress

@Component({
  template: `
    <input [formControl]="nameControl">
    
    <p>Touched: {{ nameControl.touched }}</p>
    <p>Dirty: {{ nameControl.dirty }}</p>
    <p>Valid: {{ nameControl.valid }}</p>
    <p>Value: {{ nameControl.value }}</p>
    
    <!-- Conditional styling -->
    <input 
      [formControl]="nameControl"
      [class.is-valid]="nameControl.valid && nameControl.touched"
      [class.is-invalid]="nameControl.invalid && nameControl.touched">
  `
})
export class FormStatesComponent {
  nameControl = new FormControl('', Validators.required);
}
```

**When & Why to Use:**
- **Template-driven**: Simple forms, prototyping, minimal logic
- **Reactive**: Complex validation, dynamic forms, testing requirements
- **Custom validators**: Business-specific rules (unique username, credit card)
- **Form arrays**: Dynamic lists (multiple addresses, phone numbers)
- **Value changes**: Auto-save, dependent fields, real-time validation
- **Reactive**: Better for unit testing (synchronous, predictable)

---

## Pipes

**Technical Explanation:**
Pipes are pure functions that transform data in templates. Angular provides built-in pipes for common transformations (date, currency, uppercase, etc.) and allows custom pipes. Pure pipes execute only when input values change (efficient), while impure pipes execute on every change detection cycle (use sparingly). Pipes can be chained and accept parameters.

**Metaphor:**
Pipes are like Instagram filters for your data. You have raw data (original photo), apply a pipe (filter), and get transformed output (filtered photo). The DatePipe is like a "vintage" filter that formats dates beautifully. The UpperCasePipe is like a "bold" filter that makes text stand out. You can stack multiple filters (pipes) for combined effects.

**Code Examples:**

### Built-in Pipes

```typescript
@Component({
  selector: 'app-pipes-demo',
  template: `
    <h2>Built-in Pipes</h2>
    
    <!-- Date Pipe -->
    <p>Default date: {{ today | date }}</p>
    <p>Custom format: {{ today | date:'fullDate' }}</p>
    <p>Short: {{ today | date:'short' }}</p>
    <p>Custom: {{ today | date:'dd/MM/yyyy HH:mm' }}</p>
    
    <!-- Currency Pipe -->
    <p>Price: {{ price | currency }}</p>
    <p>EUR: {{ price | currency:'EUR' }}</p>
    <p>Symbol: {{ price | currency:'USD':'symbol':'1.2-2' }}</p>
    
    <!-- Decimal/Number Pipe -->
    <p>Pi: {{ pi | number:'1.2-5' }}</p>
    <!-- Format: minIntegerDigits.minFractionDigits-maxFractionDigits -->
    
    <!-- Percent Pipe -->
    <p>Progress: {{ progress | percent }}</p>
    <p>Custom: {{ progress | percent:'1.2-2' }}</p>
    
    <!-- String Pipes -->
    <p>Uppercase: {{ name | uppercase }}</p>
    <p>Lowercase: {{ name | lowercase }}</p>
    <p>Titlecase: {{ description | titlecase }}</p>
    
    <!-- Slice Pipe -->
    <p>First 3: {{ heroes | slice:0:3 | json }}</p>
    <p>Last 2: {{ heroes | slice:-2 | json }}</p>
    
    <!-- JSON Pipe (debugging) -->
    <pre>{{ user | json }}</pre>
    
    <!-- Async Pipe (with Observables) -->
    <p>Live time: {{ time$ | async | date:'medium' }}</p>
    
    <!-- Chaining Pipes -->
    <p>{{ name | lowercase | titlecase }}</p>
    <p>{{ price | currency:'USD' | slice:0:5 }}</p>
  `
})
export class PipesDemoComponent {
  today = new Date();
  price = 1234.56;
  pi = 3.14159265359;
  progress = 0.759;
  name = 'SPIDER-MAN';
  description = 'the amazing spider-man';
  heroes = ['Iron Man', 'Thor', 'Hulk', 'Black Widow', 'Hawkeye'];
  user = { name: 'Peter Parker', age: 25, city: 'New York' };
  time$ = interval(1000).pipe(map(() => new Date()));
}
```

### Custom Pipe - Pure

```typescript
// exponential.pipe.ts
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'exponential',
  pure: true  // Default: executes only when input changes
})
export class ExponentialPipe implements PipeTransform {
  transform(value: number, exponent: number = 1): number {
    return Math.pow(value, exponent);
  }
}

// Usage:
// {{ 2 | exponential }}       → 2
// {{ 2 | exponential:3 }}     → 8
// {{ 5 | exponential:2 }}     → 25
```

### Custom Pipe - Filter Example

```typescript
// filter.pipe.ts
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'filter'
})
export class FilterPipe implements PipeTransform {
  transform(items: any[], searchText: string, property: string): any[] {
    if (!items || !searchText) {
      return items;
    }
    
    searchText = searchText.toLowerCase();
    
    return items.filter(item => {
      return item[property].toLowerCase().includes(searchText);
    });
  }
}

// Component usage:
@Component({
  template: `
    <input [(ngModel)]="searchTerm" placeholder="Search heroes">
    
    <ul>
      <li *ngFor="let hero of heroes | filter:searchTerm:'name'">
        {{ hero.name }}
      </li>
    </ul>
  `
})
export class HeroSearchComponent {
  searchTerm = '';
  heroes = [
    { name: 'Iron Man', power: 'Technology' },
    { name: 'Thor', power: 'Lightning' },
    { name: 'Hulk', power: 'Strength' }
  ];
}
```

### Custom Pipe - Async Processing

```typescript
// time-ago.pipe.ts
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'timeAgo'
})
export class TimeAgoPipe implements PipeTransform {
  transform(value: Date | string): string {
    if (!value) return '';
    
    const date = new Date(value);
    const now = new Date();
    const seconds = Math.floor((now.getTime() - date.getTime()) / 1000);
    
    const intervals: { [key: string]: number } = {
      year: 31536000,
      month: 2592000,
      week: 604800,
      day: 86400,
      hour: 3600,
      minute: 60,
      second: 1
    };
    
    for (const key in intervals) {
      const interval = Math.floor(seconds / intervals[key]);
      if (interval >= 1) {
        return interval === 1 
          ? `${interval} ${key} ago`
          : `${interval} ${key}s ago`;
      }
    }
    
    return 'just now';
  }
}

// Usage:
// {{ post.createdAt | timeAgo }}  → "5 minutes ago"
```

### Impure Pipe (Use Cautiously)

```typescript
// impure-filter.pipe.ts
@Pipe({
  name: 'impureFilter',
  pure: false  // Executes on every change detection
})
export class ImpureFilterPipe implements PipeTransform {
  transform(items: any[], filter: any): any[] {
    if (!items || !filter) {
      return items;
    }
    // This runs on EVERY change detection cycle
    return items.filter(item => item.status === filter);
  }
}

// Note: Impure pipes can impact performance
// Use pure pipes with immutable data patterns instead
```

### Parameterized Pipe

```typescript
// truncate.pipe.ts
@Pipe({
  name: 'truncate'
})
export class TruncatePipe implements PipeTransform {
  transform(
    value: string, 
    limit: number = 50, 
    ellipsis: string = '...'
  ): string {
    if (!value) return '';
    
    if (value.length <= limit) {
      return value;
    }
    
    return value.substring(0, limit) + ellipsis;
  }
}

// Usage:
// {{ longText | truncate }}              → Default 50 chars
// {{ longText | truncate:100 }}          → 100 chars
// {{ longText | truncate:20:'…' }}       → 20 chars with custom ellipsis
```

**When & Why to Use:**
- **Built-in pipes**: Common transformations (dates, currency, formatting)
- **DatePipe**: Displaying timestamps, formatting dates
- **CurrencyPipe**: E-commerce, financial apps
- **AsyncPipe**: Automatic subscription management for Observables
- **Custom pipes**: Domain-specific transformations (truncate, highlight, sanitize)
- **Pure pipes**: Performance-critical transformations (default choice)
- **Impure pipes**: When you need to transform mutable objects (rare, use sparingly)

---

## Lifecycle Hooks

**Technical Explanation:**
Lifecycle hooks are methods that Angular calls at specific moments in a component's lifecycle, from creation to destruction. They allow you to tap into key moments: initialization, change detection, view rendering, and cleanup. Implementing lifecycle interfaces (OnInit, OnDestroy, etc.) helps TypeScript catch errors and makes intentions clear.

**Metaphor:**
Lifecycle hooks are like event notifications for different stages of a person's life. Birth (constructor), first day of school (ngOnInit), every birthday (ngOnChanges), graduating (ngAfterViewInit), retirement party (ngOnDestroy). Each event lets you do specific things at the right time—you wouldn't plan retirement at birth, just like you shouldn't do heavy initialization in the constructor.

**Code Examples:**

### Complete Lifecycle Demo

```typescript
import { 
  Component, 
  OnInit, 
  OnChanges, 
  DoCheck,
  AfterContentInit,
  AfterContentChecked,
  AfterViewInit,
  AfterViewChecked,
  OnDestroy,
  Input,
  SimpleChanges
} from '@angular/core';

@Component({
  selector: 'app-lifecycle',
  template: `
    <h3>{{ title }}</h3>
    <p>Count: {{ count }}</p>
    <button (click)="increment()">Increment</button>
  `
})
export class LifecycleComponent implements 
  OnInit, OnChanges, DoCheck, AfterContentInit, AfterContentChecked,
  AfterViewInit, AfterViewChecked, OnDestroy {
  
  @Input() title = '';
  count = 0;
  
  // 1. Constructor - Called first, before any lifecycle hooks
  constructor() {
    console.log('1. Constructor called');
    // Use for: Dependency injection only
    // Avoid: Heavy initialization, API calls, DOM manipulation
  }
  
  // 2. ngOnChanges - Called when @Input properties change
  ngOnChanges(changes: SimpleChanges): void {
    console.log('2. ngOnChanges called', changes);
    
    if (changes['title']) {
      console.log('Title changed from', 
        changes['title'].previousValue, 
        'to', 
        changes['title'].currentValue);
    }
    // Use for: Reacting to input property changes
  }
  
  // 3. ngOnInit - Called once after first ngOnChanges
  ngOnInit(): void {
    console.log('3. ngOnInit called');
    // Use for: Component initialization, API calls, subscriptions
    // This is where most initialization should happen
    this.loadData();
  }
  
  // 4. ngDoCheck - Called during every change detection run
  ngDoCheck(): void {
    console.log('4. ngDoCheck called');
    // Use for: Custom change detection (use sparingly - performance)
    // Avoid: Heavy computations (called frequently)
  }
  
  // 5. ngAfterContentInit - Called once after content projection
  ngAfterContentInit(): void {
    console.log('5. ngAfterContentInit called');
    // Use for: Accessing <ng-content> projected content
  }
  
  // 6. ngAfterContentChecked - Called after every content check
  ngAfterContentChecked(): void {
    console.log('6. ngAfterContentChecked called');
    // Use for: Responding to projected content changes
  }
  
  // 7. ngAfterViewInit - Called once after view initialization
  ngAfterViewInit(): void {
    console.log('7. ngAfterViewInit called');
    // Use for: Accessing @ViewChild, DOM manipulation, third-party libs
  }
  
  // 8. ngAfterViewChecked - Called after every view check
  ngAfterViewChecked(): void {
    console.log('8. ngAfterViewChecked called');
    // Use for: Responding to view changes
  }
  
  // 9. ngOnDestroy - Called just before component destruction
  ngOnDestroy(): void {
    console.log('9. ngOnDestroy called');
    // Use for: Cleanup - unsubscribe, detach event handlers, stop timers
  }
  
  increment(): void {
    this.count++;
  }
  
  loadData(): void {
    // Simulated data loading
    console.log('Loading data...');
  }
}
```

### Practical Lifecycle Usage

```typescript
// parent.component.ts
@Component({
  selector: 'app-parent',
  template: `
    <button (click)="toggleChild()">Toggle Child</button>
    <app-child *ngIf="showChild" [message]="parentMessage"></app-child>
    <button (click)="updateMessage()">Update Message</button>
  `
})
export class ParentComponent {
  showChild = true;
  parentMessage = 'Hello from parent';
  
  toggleChild(): void {
    this.showChild = !this.showChild;
  }
  
  updateMessage(): void {
    this.parentMessage = `Updated at ${new Date().toLocaleTimeString()}`;
  }
}

// child.component.ts
@Component({
  selector: 'app-child',
  template: `<p>{{ message }}</p>`
})
export class ChildComponent implements OnInit, OnChanges, OnDestroy {
  @Input() message = '';
  private subscription!: Subscription;
  private intervalId: any;
  
  ngOnChanges(changes: SimpleChanges): void {
    // React to input changes
    if (changes['message']) {
      console.log('Message updated:', changes['message'].currentValue);
    }
  }
  
  ngOnInit(): void {
    // Set up subscriptions
    this.subscription = interval(5000).subscribe(() => {
      console.log('Periodic task running...');
    });
    
    // Set up timers
    this.intervalId = setInterval(() => {
      console.log('Timer tick');
    }, 1000);
  }
  
  ngOnDestroy(): void {
    // CRITICAL: Clean up to prevent memory leaks
    if (this.subscription) {
      this.subscription.unsubscribe();
      console.log('Subscription cleaned up');
    }
    
    if (this.intervalId) {
      clearInterval(this.intervalId);
      console.log('Timer cleaned up');
    }
  }
}
```

### ViewChild and AfterViewInit

```typescript
@Component({
  selector: 'app-view-child-demo',
  template: `
    <input #nameInput type="text">
    <button (click)="focusInput()">Focus Input</button>
    
    <app-alert #alertComponent></app-alert>
    <button (click)="showAlert()">Show Alert</button>
  `
})
export class ViewChildDemoComponent implements AfterViewInit {
  @ViewChild('nameInput') nameInput!: ElementRef;
  @ViewChild('alertComponent') alertComponent!: AlertComponent;
  
  ngAfterViewInit(): void {
    // Now we can safely access child elements and components
    console.log('Input element:', this.nameInput.nativeElement);
    
    // Auto-focus the input
    this.nameInput.nativeElement.focus();
  }
  
  focusInput(): void {
    this.nameInput.nativeElement.focus();
  }
  
  showAlert(): void {
    this.alertComponent.show('Hello from parent!');
  }
}
```

**When & Why to Use:**
- **constructor**: Dependency injection only
- **ngOnInit**: Initialization, API calls, setup (most common)
- **ngOnChanges**: React to @Input changes, re-fetch data based on inputs
- **ngAfterViewInit**: Access DOM, initialize third-party libraries (charts, maps)
- **ngOnDestroy**: Unsubscribe, cleanup, prevent memory leaks (CRITICAL)
- **ngDoCheck**: Custom change detection (rare, performance-sensitive)
- **ngAfterContentInit/Checked**: Working with projected content (ng-content)

---

## HTTP Client

**Technical Explanation:**
HttpClient is Angular's service for making HTTP requests to backend services. It's built on Observables from RxJS, provides a simplified API for GET, POST, PUT, DELETE operations, supports request/response interceptors for cross-cutting concerns (auth, logging), automatic JSON parsing, and typed responses. It requires HttpClientModule to be imported.

**Metaphor:**
HttpClient is like a postal service for your app. Your component writes a letter (request) with an address (URL) and message (data). The postal service (HttpClient) delivers it to the recipient (API server). The server sends back a reply (response), which the postal service delivers to your mailbox (Observable subscription). Interceptors are like postal inspectors who can examine and modify letters before sending or after receiving.

**Code Examples:**

### Basic Setup

```typescript
// app.module.ts
import { HttpClientModule } from '@angular/common/http';

@NgModule({
  imports: [
    HttpClientModule  // Required for HttpClient
  ]
})
export class AppModule { }
```

### Basic HTTP Operations

```typescript
// hero-http.service.ts
import { Injectable } from '@angular/core';
import { HttpClient, HttpHeaders, HttpParams, HttpErrorResponse } from '@angular/common/http';
import { Observable, throwError } from 'rxjs';
import { catchError, retry, map, tap } from 'rxjs/operators';

export interface Hero {
  id: number;
  name: string;
  power: string;
}

@Injectable({
  providedIn: 'root'
})
export class HeroHttpService {
  private apiUrl = 'https://api.example.com/heroes';
  
  constructor(private http: HttpClient) {}
  
  // GET - Fetch all heroes
  getHeroes(): Observable<Hero[]> {
    return this.http.get<Hero[]>(this.apiUrl).pipe(
      tap(heroes => console.log('Fetched heroes:', heroes)),
      catchError(this.handleError)
    );
  }
  
  // GET - Fetch single hero by ID
  getHero(id: number): Observable<Hero> {
    const url = `${this.apiUrl}/${id}`;
    return this.http.get<Hero>(url).pipe(
      tap(hero => console.log(`Fetched hero: ${hero.name}`)),
      catchError(this.handleError)
    );
  }
  
  // POST - Create new hero
  createHero(hero: Hero): Observable<Hero> {
    const headers = new HttpHeaders({
      'Content-Type': 'application/json'
    });
    
    return this.http.post<Hero>(this.apiUrl, hero, { headers }).pipe(
      tap(newHero => console.log('Created hero:', newHero)),
      catchError(this.handleError)
    );
  }
  
  // PUT - Update existing hero
  updateHero(hero: Hero): Observable<Hero> {
    const url = `${this.apiUrl}/${hero.id}`;
    
    return this.http.put<Hero>(url, hero).pipe(
      tap(updated => console.log('Updated hero:', updated)),
      catchError(this.handleError)
    );
  }
  
  // PATCH - Partial update
  patchHero(id: number, changes: Partial<Hero>): Observable<Hero> {
    const url = `${this.apiUrl}/${id}`;
    
    return this.http.patch<Hero>(url, changes).pipe(
      tap(updated => console.log('Patched hero:', updated)),
      catchError(this.handleError)
    );
  }
  
  // DELETE - Remove hero
  deleteHero(id: number): Observable<void> {
    const url = `${this.apiUrl}/${id}`;
    
    return this.http.delete<void>(url).pipe(
      tap(() => console.log(`Deleted hero: ${id}`)),
      catchError(this.handleError)
    );
  }
  
  // GET with query parameters
  searchHeroes(term: string, limit: number = 10): Observable<Hero[]> {
    const params = new HttpParams()
      .set('search', term)
      .set('limit', limit.toString());
    
    return this.http.get<Hero[]>(this.apiUrl, { params }).pipe(
      catchError(this.handleError)
    );
  }
  
  // Error handling
  private handleError(error: HttpErrorResponse): Observable<never> {
    let errorMessage = 'An error occurred';
    
    if (error.error instanceof ErrorEvent) {
      // Client-side error
      errorMessage = `Client Error: ${error.error.message}`;
    } else {
      // Server-side error
      errorMessage = `Server Error: ${error.status} - ${error.message}`;
    }
    
    console.error(errorMessage);
    return throwError(() => new Error(errorMessage));
  }
}
```

### Using HTTP Service in Component

```typescript
// hero-list.component.ts
import { Component, OnInit, OnDestroy } from '@angular/core';
import { HeroHttpService, Hero } from '../services/hero-http.service';
import { Subscription } from 'rxjs';

@Component({
  selector: 'app-hero-list',
  template: `
    <div *ngIf="loading">Loading heroes...</div>
    <div *ngIf="error" class="error">{{ error }}</div>
    
    <div *ngIf="!loading && !error">
      <h2>Heroes</h2>
      
      <!-- Search -->
      <input 
        [(ngModel)]="searchTerm" 
        (input)="search()"
        placeholder="Search heroes">
      
      <!-- Hero List -->
      <ul>
        <li *ngFor="let hero of heroes">
          {{ hero.name }} - {{ hero.power }}
          <button (click)="edit(hero)">Edit</button>
          <button (click)="delete(hero.id)">Delete</button>
        </li>
      </ul>
      
      <!-- Add New Hero -->
      <div>
        <h3>Add New Hero</h3>
        <input [(ngModel)]="newHero.name" placeholder="Name">
        <input [(ngModel)]="newHero.power" placeholder="Power">
        <button (click)="addHero()">Add</button>
      </div>
    </div>
  `,
  styles: [`
    .error { color: red; padding: 10px; background: #ffebee; }
  `]
})
export class HeroListComponent implements OnInit, OnDestroy {
  heroes: Hero[] = [];
  loading = false;
  error = '';
  searchTerm = '';
  newHero: Partial<Hero> = { name: '', power: '' };
  
  private subscriptions: Subscription[] = [];
  
  constructor(private heroService: HeroHttpService) {}
  
  ngOnInit(): void {
    this.loadHeroes();
  }
  
  loadHeroes(): void {
    this.loading = true;
    this.error = '';
    
    const sub = this.heroService.getHeroes().subscribe({
      next: (heroes) => {
        this.heroes = heroes;
        this.loading = false;
      },
      error: (err) => {
        this.error = err.message;
        this.loading = false;
      }
    });
    
    this.subscriptions.push(sub);
  }
  
  search(): void {
    if (this.searchTerm.length < 2) {
      this.loadHeroes();
      return;
    }
    
    const sub = this.heroService.searchHeroes(this.searchTerm).subscribe({
      next: (heroes) => this.heroes = heroes,
      error: (err) => this.error = err.message
    });
    
    this.subscriptions.push(sub);
  }
  
  addHero(): void {
    if (!this.newHero.name || !this.newHero.power) {
      alert('Please fill all fields');
      return;
    }
    
    const hero: Hero = {
      id: Date.now(),
      name: this.newHero.name,
      power: this.newHero.power
    };
    
    const sub = this.heroService.createHero(hero).subscribe({
      next: (created) => {
        this.heroes.push(created);
        this.newHero = { name: '', power: '' };
      },
      error: (err) => this.error = err.message
    });
    
    this.subscriptions.push(sub);
  }
  
  edit(hero: Hero): void {
    const updatedHero = { ...hero, power: hero.power + ' (Updated)' };
    
    const sub = this.heroService.updateHero(updatedHero).subscribe({
      next: (updated) => {
        const index = this.heroes.findIndex(h => h.id === updated.id);
        if (index !== -1) {
          this.heroes[index] = updated;
        }
      },
      error: (err) => this.error = err.message
    });
    
    this.subscriptions.push(sub);
  }
  
  delete(id: number): void {
    if (!confirm('Are you sure?')) return;
    
    const sub = this.heroService.deleteHero(id).subscribe({
      next: () => {
        this.heroes = this.heroes.filter(h => h.id !== id);
      },
      error: (err) => this.error = err.message
    });
    
    this.subscriptions.push(sub);
  }
  
  ngOnDestroy(): void {
    // Unsubscribe all to prevent memory leaks
    this.subscriptions.forEach(sub => sub.unsubscribe());
  }
}
```

### HTTP Interceptors

```typescript
// auth.interceptor.ts
import { Injectable } from '@angular/core';
import {
  HttpInterceptor,
  HttpRequest,
  HttpHandler,
  HttpEvent,
  HttpErrorResponse
} from '@angular/common/http';
import { Observable, throwError } from 'rxjs';
import { catchError, retry } from 'rxjs/operators';
import { Router } from '@angular/router';

@Injectable()
export class AuthInterceptor implements HttpInterceptor {
  constructor(private router: Router) {}
  
  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    // Clone and modify request - add auth token
    const token = localStorage.getItem('auth_token');
    
    let authReq = req;
    if (token) {
      authReq = req.clone({
        setHeaders: {
          Authorization: `Bearer ${token}`,
          'X-Custom-Header': 'custom-value'
        }
      });
    }
    
    // Pass request and handle errors
    return next.handle(authReq).pipe(
      retry(1),  // Retry failed request once
      catchError((error: HttpErrorResponse) => {
        if (error.status === 401) {
          // Unauthorized - redirect to login
          this.router.navigate(['/login']);
        }
        return throwError(() => error);
      })
    );
  }
}

// logging.interceptor.ts
@Injectable()
export class LoggingInterceptor implements HttpInterceptor {
  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    const startTime = Date.now();
    
    console.log(`Starting request: ${req.method} ${req.url}`);
    
    return next.handle(req).pipe(
      tap({
        next: (event) => {
          if (event.type === HttpEventType.Response) {
            const duration = Date.now() - startTime;
            console.log(`Request completed in ${duration}ms`);
          }
        },
        error: (error) => {
          const duration = Date.now() - startTime;
          console.error(`Request failed after ${duration}ms:`, error);
        }
      })
    );
  }
}

// Register interceptors in app.module.ts
import { HTTP_INTERCEPTORS } from '@angular/common/http';

@NgModule({
  providers: [
    {
      provide: HTTP_INTERCEPTORS,
      useClass: AuthInterceptor,
      multi: true  // Allow multiple interceptors
    },
    {
      provide: HTTP_INTERCEPTORS,
      useClass: LoggingInterceptor,
      multi: true
    }
  ]
})
export class AppModule { }
```

### Advanced HTTP Features

```typescript
// Advanced HTTP techniques
@Injectable({
  providedIn: 'root'
})
export class AdvancedHttpService {
  constructor(private http: HttpClient) {}
  
  // Upload file with progress tracking
  uploadFile(file: File): Observable<HttpEvent<any>> {
    const formData = new FormData();
    formData.append('file', file);
    
    return this.http.post('/api/upload', formData, {
      reportProgress: true,
      observe: 'events'
    });
  }
  
  // Download file as blob
  downloadFile(filename: string): Observable<Blob> {
    return this.http.get(`/api/files/${filename}`, {
      responseType: 'blob'
    });
  }
  
  // Get full HTTP response (not just body)
  getFullResponse(): Observable<HttpResponse<any>> {
    return this.http.get('/api/data', {
      observe: 'response'
    });
  }
  
  // Custom response type
  getTextContent(): Observable<string> {
    return this.http.get('/api/text', {
      responseType: 'text'
    });
  }
  
  // Parallel requests with forkJoin
  loadMultipleResources(): Observable<any[]> {
    return forkJoin([
      this.http.get('/api/heroes'),
      this.http.get('/api/villains'),
      this.http.get('/api/battles')
    ]);
  }
  
  // Sequential requests with concatMap
  createUserAndProfile(user: any): Observable<any> {
    return this.http.post('/api/users', user).pipe(
      concatMap(createdUser => 
        this.http.post('/api/profiles', {
          userId: createdUser.id,
          ...user.profile
        })
      )
    );
  }
  
  // Caching with shareReplay
  private cachedHeroes$: Observable<Hero[]> | null = null;
  
  getCachedHeroes(): Observable<Hero[]> {
    if (!this.cachedHeroes$) {
      this.cachedHeroes$ = this.http.get<Hero[]>('/api/heroes').pipe(
        shareReplay(1)  // Cache and share result
      );
    }
    return this.cachedHeroes$;
  }
  
  // Refresh cache
  refreshCache(): void {
    this.cachedHeroes$ = null;
  }
}
```

### Handling Different Response Types

```typescript
// file-handler.component.ts
@Component({
  selector: 'app-file-handler',
  template: `
    <input type="file" (change)="onFileSelected($event)">
    <button (click)="upload()" [disabled]="!selectedFile">Upload</button>
    
    <div *ngIf="uploadProgress">
      Upload Progress: {{ uploadProgress }}%
    </div>
    
    <button (click)="download()">Download File</button>
  `
})
export class FileHandlerComponent {
  selectedFile: File | null = null;
  uploadProgress = 0;
  
  constructor(private advancedService: AdvancedHttpService) {}
  
  onFileSelected(event: any): void {
    this.selectedFile = event.target.files[0];
  }
  
  upload(): void {
    if (!this.selectedFile) return;
    
    this.advancedService.uploadFile(this.selectedFile).subscribe({
      next: (event) => {
        if (event.type === HttpEventType.UploadProgress) {
          this.uploadProgress = Math.round(100 * event.loaded / event.total!);
        } else if (event.type === HttpEventType.Response) {
          console.log('Upload complete:', event.body);
          this.uploadProgress = 0;
        }
      },
      error: (err) => console.error('Upload failed:', err)
    });
  }
  
  download(): void {
    this.advancedService.downloadFile('report.pdf').subscribe({
      next: (blob) => {
        // Create download link
        const url = window.URL.createObjectURL(blob);
        const link = document.createElement('a');
        link.href = url;
        link.download = 'report.pdf';
        link.click();
        window.URL.revokeObjectURL(url);
      },
      error: (err) => console.error('Download failed:', err)
    });
  }
}
```

**When & Why to Use:**
- **GET**: Fetching data (lists, details, search results)
- **POST**: Creating new resources (registration, form submission)
- **PUT**: Complete update of resources
- **PATCH**: Partial update (changing single fields)
- **DELETE**: Removing resources
- **Interceptors**: Authentication, logging, error handling, request modification
- **Query params**: Filtering, pagination, sorting
- **Headers**: Authentication tokens, content types, custom metadata
- **Error handling**: User-friendly messages, retry logic, fallback data
- **Progress tracking**: File uploads, long-running operations
- **Caching**: Reducing server load, improving performance

---

## Summary & Best Practices

### Key Concepts Recap

1. **Components**: Building blocks of UI, contain logic, template, and styles
2. **Modules**: Organize app into cohesive feature blocks
3. **Data Binding**: Connect component data with templates
4. **Directives**: Modify DOM structure (*ngIf, *ngFor) or behavior (ngClass)
5. **Services & DI**: Share logic and data, promote reusability
6. **Routing**: Navigate between views in SPAs
7. **Observables**: Handle async operations reactively
8. **Forms**: Template-driven (simple) or Reactive (complex)
9. **Pipes**: Transform display data in templates
10. **Lifecycle Hooks**: Execute code at specific component moments
11. **HttpClient**: Communicate with backend APIs

### Best Practices

#### Component Design
- **Keep components small and focused** (Single Responsibility)
- **Use OnPush change detection** for better performance
- **Avoid logic in templates** - move to component class
- **Use smart/dumb component pattern**: Smart components handle data, dumb components just display

#### Services
- **Always provide at root level** unless you need multiple instances
- **Keep services stateless** when possible
- **One service, one purpose** (separate API service from state management)

#### Observables & Subscriptions
- **Always unsubscribe** in ngOnDestroy to prevent memory leaks
- **Use async pipe** in templates (automatic unsubscribe)
- **Prefer RxJS operators** over imperative code
- **Use Subject for event bus** patterns

#### Forms
- **Use Reactive Forms** for anything beyond simple forms
- **Create custom validators** for business logic
- **Group related fields** with FormGroup
- **Provide clear validation messages**

#### Performance
- **Use trackBy** with *ngFor for large lists
- **Lazy load** feature modules
- **Use OnPush** change detection strategy
- **Avoid function calls** in templates
- **Implement virtual scrolling** for long lists

#### Code Organization
```
src/
├── app/
│   ├── core/              # Singleton services, guards
│   │   ├── services/
│   │   ├── guards/
│   │   └── interceptors/
│   ├── shared/            # Shared components, directives, pipes
│   │   ├── components/
│   │   ├── directives/
│   │   └── pipes/
│   ├── features/          # Feature modules
│   │   ├── heroes/
│   │   │   ├── components/
│   │   │   ├── services/
│   │   │   ├── models/
│   │   │   └── heroes.module.ts
│   │   └── villains/
│   └── app.module.ts
```

#### Security
- **Sanitize user input** (Angular does this by default)
- **Use HttpOnly cookies** for tokens
- **Implement route guards** for authentication
- **Never expose sensitive data** in client code
- **Validate on server-side** - never trust client

#### Testing
- **Write unit tests** for services and components
- **Use TestBed** for Angular-specific testing
- **Mock HTTP calls** with HttpTestingController
- **Test user interactions** with component testing
- **E2E tests** for critical user flows

### Common Pitfalls to Avoid

1. **Subscribing without unsubscribing** → Memory leaks
2. **Heavy logic in constructor** → Use ngOnInit instead
3. **Modifying input properties** → Use @Output or services
4. **Not using TypeScript types** → Loses type safety benefits
5. **Too many subscriptions** → Use async pipe or combine observables
6. **Mutating objects** → Use immutable patterns
7. **Not handling errors** → Always have error handlers
8. **Ignoring Angular style guide** → Inconsistent codebase

### Next Steps

After mastering these concepts:
1. **State Management**: Learn NgRx or Akita for complex apps
2. **Testing**: Jasmine, Karma, Protractor/Cypress
3. **Advanced RxJS**: Custom operators, subjects, schedulers
4. **Performance**: AOT compilation, tree-shaking, bundle optimization
5. **PWA**: Service workers, offline support
6. **Server-Side Rendering**: Angular Universal for SEO
7. **Micro-frontends**: Module Federation, monorepo architectures

### Resources
- **Official Docs**: https://angular.io/docs
- **Angular Blog**: https://blog.angular.io
- **Style Guide**: https://angular.io/guide/styleguide
- **RxJS Docs**: https://rxjs.dev
- **Community**: Stack Overflow, Reddit r/Angular

---

## Quick Reference Cheat Sheet

```typescript
// Component
@Component({
  selector: 'app-example',
  template: `<h1>{{ title }}</h1>`,
  styles: [`h1 { color: blue; }`]
})
export class ExampleComponent {}

// Service
@Injectable({ providedIn: 'root' })
export class DataService {}

// Directive
@Directive({ selector: '[appHighlight]' })
export class HighlightDirective {}

// Pipe
@Pipe({ name: 'custom' })
export class CustomPipe implements PipeTransform {
  transform(value: any): any { return value; }
}

// Module
@NgModule({
  declarations: [/* components, directives, pipes */],
  imports: [/* other modules */],
  providers: [/* services */],
  bootstrap: [/* root component */]
})
export class AppModule {}

// Data Binding
{{ expression }}              // Interpolation
[property]="value"           // Property binding
(event)="handler()"          // Event binding
[(ngModel)]="property"       // Two-way binding

// Directives
*ngIf="condition"
*ngFor="let item of items"
*ngSwitch / *ngSwitchCase
[ngClass]="classes"
[ngStyle]="styles"

// Forms
FormControl / FormGroup / FormArray
Validators.required / email / pattern / min / max

// HTTP
this.http.get<T>(url)
this.http.post<T>(url, data)
this.http.put<T>(url, data)
this.http.delete<T>(url)

// Routing
{ path: 'heroes', component: HeroesComponent }
<router-outlet></router-outlet>
<a routerLink="/heroes">Heroes</a>
this.router.navigate(['/heroes', id])

// Lifecycle
ngOnInit() / ngOnDestroy() / ngOnChanges()
ngAfterViewInit() / ngAfterContentInit()
```

---

## Llinks
1. https://medium.com/williambastidasblog/mastering-angular-understanding-the-core-concepts-b12620d68dbe
2. https://medium.com/@Rushabh_/angular-basic-to-advance-every-concept-explained-part-1-e8f8692e04b3
3. https://medium.com/@schaman762/if-you-can-answer-these-8-angular-concepts-correctly-youre-decent-engineer-1bf26f1e6877
4. https://iammadhankumar.medium.com/angular-must-know-advanced-concepts-c46078e79a52
