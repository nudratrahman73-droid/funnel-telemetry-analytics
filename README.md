# Funnel Telemetry Analytics (App Insights → Azure → Power BI)

## Problem
Marketing wanted to see where users drop off from a campaign funnel:
**Page view → Event1 → Event2**, with conversion % over time.

## Data
- Source: Azure **Application Insights** (JSONL) – `pageViews`, `customEvents`
- Grain: **session_id**; keys align across page views/events
- Scope: last 24h / 30d / 90d via relative date

## Architecture
App Insights (export to storage) → Synapse serverless (OPENROWSET) → SQL table (`dbo.AppEvents`) → **Power BI** (DAX measures + RLS).  


## Model (Star Schema)
Facts: `AppEvents` (session_id, page_name, event_name, ts)  
Dims: `DimDate`, `DimEvent`, optional `DimCampaign`.  
Measures compute **Event1 sessions, Event2 sessions, Conversion%**.

## Results
- Interactive **funnel** with % of first stage & counts per event
- **Time window** slicer (24h/30d/90d)
- **RLS** by user group to limit visible events/pages
