# ZeroClient MVP initial concept - 2025-05-01 - 2025-05-03

## 1. Key Problem Identification

Effective (1) monitoring of multiple (2) heterogeneous (3) industrial installations (4) in mobile (5), bandwidth-constrained (6), stability-challenged (7), and economically-sensitive (8) Internet access conditions, while minimizing dependence on the deployment status (9) of the hardware and software layer on individual installations.

(1) Effective - focusing on operational efficiency, including on mobile devices

(2) Multiple - working in an N-to-M model - many users with differently configured applications monitor many installations

(3) Heterogeneous - primarily installations monitored via ZeroSCADA, but immediately opening up to ThingsBoard instances as well, and potentially other sources via generic / custom microfrontends + appropriate modules in the ZeroClient backend

(4) Industrial - so even on a micro-scale, reliability and trustworthiness are priorities

(5) Mobile - so the web application must be conveniently usable on a phone, and we ultimately want a native application

(6) Low bandwidth connections - at least sometimes, so efficient and conveniently controlled element caching, preferably with visualization of what we have in the cache, manual control of caching, deleting / refreshing cache, control of "what's missing"

(7) Problematic connection stability - so all components must be ready for such conditions, plus it's good to place a communication status indicator with the server somewhere (per installation) and the time of the last contact

(8) Economic transmission limitations - primarily roaming, paid connections, packages, but this aspect is largely handled by the mobile devices themselves, so it's mainly about sensitizing the application to information flowing from the system, a solution similar to used in Spotify

(9) Minimizing dependence on deployment status - the point with the greatest impact on the application's operation, because it includes:

- independence from the readiness of designed / manually configured synoptic screens by introducing auto-generated text-tabular screens, with the ability to switch to graphical ones as deployment progresses

- facilities such as "autodiscovery" reducing the time to create configurations on the ZeroClient side based on the subscribed installation (ZeroSCADA / TB as a source)

- easy forcing of controls for selected variables (e.g., easy configuration for a given variable of a control screen with several buttons forcing values, e.g., "0", "1", "100", etc.)

This list will be expanded.

Furthermore, it should be emphasized:

- Client applications should be completely decoupled from the ZeroSCADA backend

- One client application should be able to subscribe to data from multiple backends

- Each backend instance should be able to handle multiple frontend instances

### Additional Requirements Outside the Basic Business Context

Products rarely arise in a "full greenfield" vacuum. Hence the existence of additional requirements / constraints, often "on the border" - a technical requirement imposed by the business (e.g., we must use the Azure cloud due to cooperation agreements with Microsoft; we must use Gemini models because we are preparing an application for a Google competition).

- Due to planned cooperation with selected partners - direction (analytics) and their preferred stack (Python) - we are CONSIDERING using Python and the Dash framework, but it is not necessary.

- Due to the assumption of high code reusability - we minimize maintenance costs - orchestration with the use of microfrontend architecture and React Native technology should be considered.

- Due to the need to provide selected visualizations to public partners, such as municipalities or universities, microfrontends should have an auto-generated lightweight version for placement as a widget on a website.

- Due to economic efficiency and the educational aspect, the basis of deployment is Mikrus 1.0 Pro (LXC container providing 1 vCPU, 640 MB RAM, 10 GB SSD, Ubuntu Linux, Docker).

- Due to the need to address the needs of individual clients as well, e.g., in public or scientific-research projects, data presentation must be very visually appealing.

### Business Value

- Enabling effective visualization of collected data

- Facilitating the deployment of the ZeroSCADA backend on facilities

- A base for cooperation with IT teams in friendly companies

## 2. Minimum Set of Functionalities

- User registration + login

- Access levels

  - **Informational:** access to collected data and its visualization

  - **Monitoring:** additionally access to detailed logs and information about problems

  - **Service:** additionally the ability to add devices and change access keys

  - **Administrative:** full control, including user management

- Subscription of at least 2 backends based on ZeroSCADA, using URL API and API key:

  - dummy (simulated) / IBI Lab (internal)

  - URHydro (actual project developed in parallel, we have some influence on its shape, e.g., on the shape of the API)

- Persistence for object subscriptions

- Management of subscribed objects:

  - Editing a friendly name

  - Deleting a subscription

- Autogenerated tabular/text views

- Basic 2 views:

  - current status, with the last measurement and time of last contact with the backend

  - simple history chart (last 12h, averaging)

- Web deployment (Docker @ Mikrus 1.0 Pro)

- Embedding a widget on a website

- One-handed operation in a browser on a smartphone

- Basic version of the cache ("cache all")

- Some features of advanced caching:

  - Downloading all necessary elements on demand
  
  - Widgets, pages, definitions, data

  - Monitoring with minimal transfer

  - Optimization for poor GSM coverage/no WiFi

## 3. What is NOT included in the MVP

- Reports

- Alarms

- React Native application on the phone

- Support for TB / generic / other instances

- Handling information about the connection on devices (WiFi / GSM)

- Detailed cache management

- Full user support (self-reset password, etc.)

- Tickets to support inside the application

- Full API autodiscovery (we provide the schema)

- Processing modules in the ZeroClient backend

- MCP and analysis of results using LLM

- Support for multiple languages - initially everything in English

## 4. Success Criteria

- The application allows subscribing to two objects: IBI Lab and URHydro

- The possibility of current viewing of the station status during installation / service of the URHydro station (poor GSM conditions, the need to work from a smartphone, not a laptop)

- Widget with URHydro measurements on my blog's website (Wordpress) / fallback to a dedicated landing page

---
Created on 2025-05-03 by Grzegorz Zieliński with assistance from Alice.
