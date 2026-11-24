# Product — Sounding Room

## What it is

Sounding Room is a browser app for interpreting Vertical Electrical Soundings (VES) using the Schlumberger electrode array. It targets Nigerian groundwater surveys — a domain that still depends on Windows 95-era tools (IPI2Win, WinResist) with no browser-based tool built for this workflow.

## Problem it solves

IPI2Win's user guide (2001) targets Windows 95 and needs a hardware license plug. Students and small firms struggle to get or run these tools on modern machines. Sounding Room replaces them with a static web app that works in any browser, with no installation and no server.

## Users

- Geophysics students learning VES interpretation, particularly in Nigerian universities.
- Small borehole-siting firms that need quick curve matching and layer inversion without a commercial licence.

## Core features (target)

- Load raw VES field data from CSV or Basiri `.DAT` files.
- Display the apparent-resistivity sounding curve on a log-log plot (AB/2 vs ρ_a).
- Identify curve type (A, K, H, Q and compound types).
- Run forward modelling: given layer thicknesses and resistivities, compute the theoretical sounding curve.
- Run inversion: fit a layered-earth model to measured data, producing layer thicknesses and resistivities with RMS misfit.
- Export results.

## Constraints

- **No server.** All computation happens in the browser. User data never leaves the device.
- **No login.** No accounts, no telemetry.

## Validation dataset

`data/basiri/` holds 30 real Schlumberger VES stations from Basiri, Ado-Ekiti, Nigeria (Aizebeokhai et al., 2018, CC BY 4.0), interpreted by the authors with WinResist. Their `.RST` models are the reference results for the validation report. The directory is read-only; nothing in `src/` may write to it.

## Hackathon context

Kiroween 2026, Resurrection category. Built with Kiro. Started 2026-11-24.
