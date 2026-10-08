# 🚨 SafeRoute Keralam
Made for keralam By a Btech Student 

## AHP-Based Emergency Vehicle Route Safety & Decision Support System

**SafeRoute Keralam** is a web-based emergency route planning and decision-support system developed to identify and evaluate safer routes during **floods, heavy rainfall, road blockages, and other emergency situations in Kerala**.

The system combines **Analytic Hierarchy Process (AHP)**, **real road-network routing**, **OpenStreetMap geographic data**, and **vehicle-specific route suitability analysis**.

It supports two major emergency operations:

- 🚑 **Ambulance → Hospital**
- 🚚 **Relief Truck → Relief Camp / Evacuation Centre**

Instead of selecting a route based only on distance, SafeRoute Keralam evaluates multiple road-safety criteria and identifies the route with the highest overall **Route Suitability Score (RSS)**.

---

## 🌐 Live Project

🔗 **GitHub Repository:**  
https://github.com/shyleshm-tech/saferoute-keralam/

---
## LINK FOR WEBSITE 
https://shyleshm-tech.github.io/saferoute-keralam/

## 🎯 Project Objective

During emergency situations, the shortest route is not always the safest or most suitable route.

For example, a shorter road may have:

- Poor road condition
- Narrow road width
- Heavy traffic
- Major road blockages
- Poor accessibility

Therefore, SafeRoute Keralam aims to answer:

> **"Which available route is the most suitable for this emergency vehicle and mission?"**

### Main objectives

- Identify real road routes between an origin and destination.
- Calculate actual road-network distance.
- Generate alternative routes.
- Evaluate road-safety characteristics.
- Apply AHP-derived criterion weights.
- Consider emergency vehicle suitability.
- Rank alternative routes.
- Recommend the most suitable route.

---

# 🚑 Emergency Vehicle Modes

## 🚑 Ambulance Mode

Ambulance mode is designed for:

```text
Patient Location
       ↓
     Hospital
```

The application can use the user's current GPS location as the starting point and identify nearby hospitals.

The route evaluation considers:

- Road distance
- Traffic congestion
- Road condition
- Road width
- Road blockages
- Destination accessibility
- Ambulance suitability

Narrow, blocked, or poor-condition roads can receive lower suitability.

---

## 🚚 Relief Truck Mode

Relief Truck mode is designed for:

```text
Dispatch Location
       ↓
Relief Camp / Evacuation Centre
```

Unlike ambulance mode, relief trucks are directed toward **relief camps or evacuation centres rather than hospitals**.

The system considers:

- Road distance
- Road width
- Road condition
- Traffic congestion
- Road blockages
- Destination accessibility
- Relief-truck suitability

Wide roads are preferred because relief vehicles may require greater road space.

> **Note:** The current implementation uses the OSRM driving routing profile and does not provide a dedicated legal heavy-truck routing profile. Therefore, relief-truck routing represents route suitability and does not guarantee vehicle weight, height, or legal-access clearance.

---

# 🧠 Methodology

The overall methodology is:

```text
                   USER
                    │
                    ▼
           Select Vehicle Type
                    │
          ┌─────────┴─────────┐
          │                   │
          ▼                   ▼
     🚑 Ambulance        🚚 Relief Truck
          │                   │
          ▼                   ▼
      Hospital          Relief Camp /
                       Evacuation Centre
          │                   │
          └─────────┬─────────┘
                    ▼
             Get Coordinates
                    │
                    ▼
          Real Road Routing
                 (OSRM)
                    │
                    ▼
          Generate Alternatives
                    │
                    ▼
            Route Attributes
                    │
                    ▼
              AHP Scoring
                    │
                    ▼
         Vehicle Suitability
                    │
                    ▼
        Route Suitability Score
                    │
                    ▼
             Route Ranking
                    │
                    ▼
        🟢 Recommended Route
```

---

# 🧠 Analytic Hierarchy Process (AHP)

The project uses **Analytic Hierarchy Process (AHP)** as the multi-criteria decision-making method.

AHP is used to determine the relative importance of different route-safety criteria.

The original project survey produced the AHP weights.

The current implementation excludes the Flooding criterion and renormalises the remaining weights.

### AHP Weights Used

| Criterion | Weight |
|---|---:|
| 📏 Distance | 0.0874145861 |
| 🚦 Traffic Congestion | 0.1584597482 |
| 🛣️ Road Condition | 0.1805917472 |
| ↔️ Road Width | 0.1089224081 |
| 🚧 Road Blockages | 0.1656752471 |
| 🏥 Destination Accessibility | 0.2989362633 |
| **Total** | **1.0000000000** |

### Consistency Ratio

The original AHP analysis produced:

```text
CR = 0.02779718
```

Approximately:

```text
CR = 0.0278
```

A consistency ratio below 0.10 is generally considered acceptable for AHP-based pairwise comparison analysis.

---

# 🧮 Route Suitability Score

Each available route receives a **Route Suitability Score (RSS)**.

The basic model is:

```text
RSS = Σ (Weight × Criterion Score)
```

The implemented model can be represented as:

```text
RSS =
(WD × Distance Score)
+
(WT × Traffic Score)
+
(WC × Condition Score)
+
(WW × Width Score)
+
(WB × Blockage Score)
+
(WA × Accessibility Score)
```

Where:

| Symbol | Criterion |
|---|---|
| WD | Distance |
| WT | Traffic Congestion |
| WC | Road Condition |
| WW | Road Width |
| WB | Road Blockages |
| WA | Destination Accessibility |

The route with the highest suitable score is ranked as the recommended route.

---

# 📊 Road Attribute Scoring

The qualitative road attributes are converted into numerical values.

## 🚦 Traffic Congestion

| Traffic Level | Score |
|---|---:|
| Low | 1.0 |
| Medium | 0.5 |
| High | 0.0 |

## 🛣️ Road Condition

| Condition | Score |
|---|---:|
| Good | 1.0 |
| Fair | 0.5 |
| Poor | 0.0 |

## ↔️ Road Width

| Width | Score |
|---|---:|
| Wide | 1.0 |
| Medium | 0.5 |
| Narrow | 0.0 |

## 🚧 Road Blockages

| Blockage | Score |
|---|---:|
| None | 1.0 |
| Minor | 0.5 |
| Major | 0.0 |

## 🏥 Destination Accessibility

| Accessibility | Score |
|---|---:|
| Direct | 1.0 |
| Moderate | 0.5 |
| Indirect | 0.0 |

---

# 🗺️ Real Road Routing

A major feature of SafeRoute Keralam is that route distance is based on the **actual road network** rather than straight-line distance.

The project uses **OSRM (Open Source Routing Machine)** for road routing.

Therefore:

```text
Straight-line Distance
        ❌
```

is not used as the actual route distance.

Instead:

```text
Origin
   ↓
Road Network
   ↓
OSRM
   ↓
Actual Road Route
   ↓
Road Distance
```

This allows the system to calculate realistic road distances and display the actual road geometry on the map.

---

# 🛣️ Alternative Routes

The system requests alternative routes from the routing service.

The alternatives are then evaluated using the project's safety criteria.

```text
Origin
  │
  ▼
Destination
  │
  ▼
OSRM
  │
  ├── Route 1
  ├── Route 2
  └── Route 3
       │
       ▼
  Safety Evaluation
       │
       ▼
  AHP Scoring
       │
       ▼
  Vehicle Suitability
       │
       ▼
  Route Ranking
```

This means the system does not simply choose the shortest road.

---

# 🚨 Vehicle Suitability

Distance is only one component of emergency route selection.

The application also considers whether the route is suitable for the selected emergency vehicle.

## 🚑 Ambulance

```text
Wide Road       → Suitable
Medium Road     → Caution
Narrow Road     → Reduced Suitability

No Blockage    → Suitable
Minor Blockage → Caution
Major Blockage → Avoid / Penalize
```

Poor road conditions can also reduce route suitability.

---

## 🚚 Relief Truck

```text
Wide Road       → Preferred
Medium Road     → Caution
Narrow Road     → Reduced Suitability

No Blockage    → Preferred
Minor Blockage → Caution
Major Blockage → Avoid / Penalize
```

The relief-truck mode prioritises roads that are operationally more suitable for larger emergency vehicles.

---

# 📍 Emergency Destination Selection

## 🚑 Ambulance

```text
GPS / Start Location
        ↓
Nearby Hospitals
        ↓
Hospital Selection
        ↓
Real Road Routes
```

## 🚚 Relief Truck

```text
GPS / Start Location
        ↓
Relief Camps /
Evacuation Centres
        ↓
Centre Selection
        ↓
Real Road Routes
```

The system uses geographic data from **OpenStreetMap-related services** to identify emergency destinations.

---

# 📁 Road Safety Dataset

The project can use an external road-safety dataset containing research-specific road attributes.

Supported formats include:

```text
.csv
.xls
.xlsx
```

Typical attributes include:

```text
Traffic Congestion
Road Condition
Road Width
Road Blockages
Destination Accessibility
```

Distance does not necessarily need to be supplied because the application can obtain actual road distance from OSRM.

---

# ❓ Why Use a Road Safety Dataset?

The routing service can determine:

```text
Where the road goes
+
How far the route is
```

However, the research model requires additional attributes such as:

- Traffic condition
- Road condition
- Road width
- Road blockage
- Accessibility

Therefore, the road dataset provides the **research-specific safety information** required by the AHP model.

The project therefore combines:

```text
REAL ROAD NETWORK
        +
ROAD SAFETY DATA
        +
AHP WEIGHTS
        ↓
ROUTE SUITABILITY
```

---

# 🗺️ GIS & Mapping

The interactive map is developed using **Leaflet.js**.

Leaflet provides:

- Interactive maps
- Origin markers
- Destination markers
- Route polylines
- Alternative route visualization
- Zooming and map navigation

The project uses **Leaflet 1.9.4**.

---

# 🌍 Geographic Data

The application uses OpenStreetMap-related geographic services for:

- Road network information
- Hospitals
- Relief camps
- Evacuation centres
- Geographic coordinates

This allows the application to work with real geographic locations rather than only predefined locations.

---

# 💻 Technology Stack

| Technology | Purpose |
|---|---|
| **HTML5** | Web application structure |
| **CSS3** | User interface and styling |
| **JavaScript** | Application logic |
| **Leaflet.js** | Interactive GIS map |
| **OSRM** | Real road routing |
| **OpenStreetMap** | Geographic/map data |
| **Overpass API** | Emergency destination discovery |
| **SheetJS** | Excel/CSV processing |
| **AHP** | Multi-criteria decision making |
| **GitHub Pages** | Web deployment |

---

# 🏗️ System Architecture

```text
┌───────────────────────────────────────────┐
│             SafeRoute Keralam             │
└─────────────────────┬─────────────────────┘
                      │
                      ▼
             ┌────────────────┐
             │  Web Interface │
             │ HTML + CSS + JS│
             └───────┬────────┘
                     │
       ┌─────────────┼─────────────┐
       │             │             │
       ▼             ▼             ▼
   Leaflet          OSRM       OpenStreetMap
     Map           Routing      / Overpass
       │             │             │
       └─────────────┼─────────────┘
                     │
                     ▼
             Alternative Routes
                     │
                     ▼
             Road Safety Data
                     │
                     ▼
                AHP Weights
                     │
                     ▼
           Vehicle Suitability
                     │
                     ▼
          Route Suitability Score
                     │
                     ▼
                Route Ranking
                     │
                     ▼
            Recommended Route
```

---

# 📂 Repository Structure

```text
saferoute-keralam/
│
├── index.html
│
└── README.md
```

The primary web application is contained in:

```text
index.html
```

---

# ▶️ Running the Project

## Method 1 — GitHub Pages

The project can be deployed using GitHub Pages.

### Steps

1. Open the repository.
2. Go to:

```text
Settings → Pages
```

3. Select:

```text
Deploy from a branch
```

4. Select:

```text
main
```

5. Select:

```text
/ (root)
```

6. Click **Save**.

GitHub Pages will generate the public website.

---

## Method 2 — Run Locally

Clone the repository:

```bash
git clone https://github.com/shyleshm-tech/saferoute-keralam.git
```

Enter the project directory:

```bash
cd saferoute-keralam
```

Then open:

```text
index.html
```

in a modern web browser.

For development, VS Code Live Server can also be used.

---

# 📱 How to Use

### 1️⃣ Select Emergency Vehicle

Choose:

```text
🚑 Ambulance
```

or:

```text
🚚 Relief Truck
```

---

### 2️⃣ Select Starting Location

Use the location interface or GPS functionality to determine the starting point.

---

### 3️⃣ Select Destination

For ambulance:

```text
🏥 Hospital
```

For relief truck:

```text
⛺ Relief Camp / Evacuation Centre
```

---

### 4️⃣ Generate Routes

The system requests real road routes through OSRM.

---

### 5️⃣ Compare Routes

Each route is evaluated using:

```text
Distance
Traffic
Road Condition
Road Width
Road Blockages
Accessibility
Vehicle Suitability
```

---

### 6️⃣ Select Recommended Route

The system ranks routes according to their calculated suitability.

The highest suitable route is highlighted as the recommended route.

---

# 📊 Example

Suppose three routes are available:

| Route | Distance | Condition | Width | Blockage | Suitability |
|---|---:|---|---|---|---:|
| Route 1 | 8.2 km | Good | Wide | None | 0.81 |
| Route 2 | 6.9 km | Fair | Medium | Minor | 0.72 |
| Route 3 | 5.8 km | Poor | Narrow | Major | 0.41 |

A conventional shortest-path system might select:

```text
Route 3
```

because it is the shortest.

SafeRoute Keralam may select:

```text
Route 1
```

because it provides a better overall safety profile.

Therefore:

> **Shortest Route ≠ Always the Most Suitable Emergency Route**

---

# 🔬 Research Contribution

SafeRoute Keralam combines:

```text
AHP
+
GIS
+
Real Road Routing
+
Road Safety Parameters
+
Vehicle Suitability
+
Emergency Destination Selection
```

into a single web-based emergency route decision-support system.

The project demonstrates how **Multi-Criteria Decision Making (MCDM)** can be combined with **GIS-based routing** for emergency transportation planning.

---

# 🌧️ Application Areas

The system can potentially support:

- Flood emergency response
- Ambulance route planning
- Relief material transportation
- Evacuation planning
- Emergency logistics
- Disaster management studies
- Monsoon road safety analysis
- GIS-based emergency planning
- Civil engineering research
- Transportation planning

---

# ⚠️ Limitations

SafeRoute Keralam is an **academic/research prototype** and should not be considered an operational emergency dispatch system.

The system cannot guarantee:

- Real-time road availability
- Flood-free roads
- Current traffic conditions
- Current road blockages
- Emergency response time
- Hospital availability
- Relief-centre availability
- Legal heavy-vehicle access
- Actual emergency outcomes

Real emergency decisions should always consider information from:

- Police
- Fire & Rescue Services
- Disaster Management Authorities
- Local Government
- Hospital authorities
- Other authorised emergency agencies

---

# 🚀 Future Scope

Future versions can include:

- 🌧️ Real-time flood-depth information
- 🚧 Live road blockage information
- 🚦 Real-time traffic data
- 🛰️ Satellite-based flood mapping
- 🏥 Live hospital capacity
- 🚑 Emergency response-time prediction
- 🚚 Dedicated heavy-vehicle routing
- 📡 Government disaster-management integration
- 🗺️ Flood-risk GIS layers
- 📊 Dynamic AHP weighting
- 🤖 Machine-learning route-risk prediction
- 📱 Mobile/PWA version
- 🔔 Emergency alerts
- 🌐 Real-time disaster dashboards

---

# 🧪 Academic Model

The decision-making process can be summarised as:

```text
Emergency Routing Problem
          │
          ▼
Multiple Route Alternatives
          │
          ▼
Multiple Safety Criteria
          │
          ▼
Analytic Hierarchy Process
          │
          ▼
Criterion Weights
          │
          ▼
Route Scoring
          │
          ▼
Vehicle Suitability
          │
          ▼
Route Suitability Score
          │
          ▼
Route Ranking
          │
          ▼
Recommended Emergency Route
```

---

# 🔑 Key Concept

SafeRoute Keralam has two major layers.

## 1. Routing Layer

Answers:

> **"Which roads connect the origin and destination?"**

This is handled through real road-network routing.

## 2. Decision Layer

Answers:

> **"Which available route is more suitable for the selected emergency mission?"**

This is handled using:

- AHP
- Road-safety criteria
- Vehicle suitability
- Route scoring

Therefore:

```text
Real Road Routing
        +
Multi-Criteria Decision Making
        +
Vehicle Suitability
        =
Emergency Route Decision Support
```

---

# 👨‍💻 Developer

## **Shylesh M Nampoothiri**

🎓 **B.Tech 3rd Year – Civil Engineering**

### Project

**SafeRoute Keralam**

A research-oriented emergency route planning and decision-support system integrating:

- 🗺️ GIS & digital mapping
- 🛣️ Real road-network routing
- 🧠 Analytic Hierarchy Process (AHP)
- 🚑 Ambulance emergency routing
- 🚚 Relief-truck routing
- 🏥 Hospital accessibility
- ⛺ Relief camp & evacuation-centre selection
- 🌧️ Monsoon/flood emergency route safety

### GitHub

🔗 https://github.com/shyleshm-tech

### Project Repository

🔗 https://github.com/shyleshm-tech/saferoute-keralam/

---

# 📜 Disclaimer

This project is developed for **academic, research, demonstration, and educational purposes**.

SafeRoute Keralam does not guarantee:

- Road availability
- Emergency response time
- Road clearance
- Flood-free travel
- Legal vehicle access
- Hospital availability
- Relief-centre availability
- Actual emergency response outcomes

During an actual emergency, users should always follow instructions from authorised emergency and disaster-management authorities.

---

# ⭐ Project Summary

**SafeRoute Keralam** demonstrates an approach to emergency route decision-making by integrating:

> 🧠 **AHP-Based Multi-Criteria Analysis**  
> 🗺️ **Real Road-Network Routing**  
> 📍 **GIS & Geographic Data**  
> 🚑 **Ambulance Routing**  
> 🚚 **Relief-Truck Routing**  
> 🏥 **Hospital Selection**  
> ⛺ **Relief Camp / Evacuation Centre Selection**  
> 📊 **Route Suitability Scoring**

The central idea is:

### **Don't just find the shortest route. Find the most suitable route for the emergency.**

---

## 🇮🇳 Developed for Emergency Route Safety Research in Kerala

**© 2026 Shylesh M Nampoothiri | B.Tech 3rd Year – Civil Engineering**



Some samples of the site 



pic 1
<img width="1281" height="872" alt="image" src="https://github.com/user-attachments/assets/d347eb7d-1017-4c13-8c25-ef97aa2bd88c" />
pic 2
<img width="1857" height="897" alt="image" src="https://github.com/user-attachments/assets/2369ada7-d2c1-4b9b-ac2e-f0637a1bc3db" />
pic 3
<img width="827" height="561" alt="image" src="https://github.com/user-attachments/assets/7dd418d5-6c21-4da8-bdcc-87f3df619573" />

# ☕ Support the Developer

If **SafeRoute Keralam** helped you, impressed you, confused you, or made you spend way too much time staring at routes on a map... 😅

You can support the developer with a cup of coffee ☕❤️

> **Every line of code was written with Civil Engineering knowledge, JavaScript debugging, and an unhealthy amount of coffee.** 😂

### ☕ Buy Me a Coffee

If you'd like to support the project:

**☕ Buy me a coffee — because even emergency routes need fuel!**

[💰 Support the Developer](#)

---

### 😂 Developer Fuel

```text
Civil Engineering        ████████████████  100%
JavaScript               ████████████      75%
GIS                      ███████████████   90%
AHP                      ██████████████    85%
Debugging                █████████████████ 110%
Coffee                   █████████████████ ∞
Sleep                    ██                  10%
```

### 👨‍💻 Built by

**Shylesh M Nampoothiri**  
B.Tech 3rd Year – Civil Engineering

> *"If the route doesn't work, check the code.  
> If the code doesn't work, check the route.  
> If both don't work... have coffee."* ☕😂

---

### ⭐ Like the Project?

If you found this project useful, consider:

⭐ **Starring the repository**  
🍴 **Forking the project**  
🐛 **Reporting bugs**  
💡 **Suggesting improvements**  
☕ **Buying the developer a coffee**

Every bit of support helps keep the project — and the developer — running! 🚀
