# ⚡ Fabric Smart EV Charger — Real-Time Carbon-Aware Charging Optimizer

A real-time data pipeline on **Microsoft Fabric** that decides *when* to charge an EV based on live UK grid carbon intensity — charging when electricity is cleanest, idling when it isn't.

## 🏗️ Architecture

<p align="center">
  <img src="screenshots/RTI EV Project.png" alt="Fabric Smart EV Charger Architecture" width="850">
</p>

## 🧠 How It Works

Every **60 seconds**, the pipeline runs:

🇬🇧 **UK National Grid API**  
↓  
🐍 **Fabric Notebook** — fetches live carbon intensity  
↓  
📈 **Rolling 20-reading average** — establishes the recent grid baseline  
↓  
🔋 **Charging Decision**

`Current < Rolling Average` → **CHARGING**  
`Current ≥ Rolling Average` → **IDLE**

↓  
⚡ **80% Battery Cap** — charging stops at the target level  
↓  
☁️ **Fabric Eventstream** — streams the decision event  
↓  
🗄️ **Fabric Eventhouse (KQL DB)** — stores and queries the events  
↓  
📊 **Real-Time Dashboard + Activator** — live monitoring and alerts

**Data source:** [UK National Grid Carbon Intensity API](https://api.carbonintensity.org.uk/intensity)

### In one line

> **Live grid data → adaptive charging decision → simulated battery → real-time streaming → KQL → dashboard & alerts**
```
UK Carbon Intensity API → Fabric Notebook (Python) → Eventstream → Eventhouse (KQL) → Real-Time Dashboard + Activator
```

## Tech Stack

Microsoft Fabric (Eventstream, Eventhouse, Real-Time Dashboard, Activator) · Python · Azure Event Hub SDK · KQL · REST API

## Live Dashboard

<p align="center"> <img src="screenshots/RTI%20Dashboard.png" alt="Fabric Real-Time Dashboard" width="850"> </p>

Auto-refreshing tiles for average intensity, simulated battery %, and a live time-series plot.

## Optimizer in Action

<p align="center"> <img src="screenshots/Battery%20(RTI).png" alt="EV Battery and Optimizer Output" width="850"> </p>

## Repository Structure

```
.
├── UK_grid_nb.ipynb       # Notebook: polls API, simulates battery, streams events
├── screenshots/
└── README.md
```

*Built to explore Microsoft Fabric's Real-Time Intelligence workload — end-to-end streaming ingestion, storage, and live decisioning.*
