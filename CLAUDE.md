# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

质谱成像空间分辨率计算器 - A web-based calculator for measuring spatial resolution in mass spectrometry imaging (MSI). The tool analyzes response intensity data across a scan line to calculate the distance (in μm) required for signal to rise from 20% to 80% of the peak value.

## Architecture

Single-file HTML application (`index.html`) with embedded CSS and JavaScript. No build process or dependencies required.

**Key Components:**
- **Data Input**: Accepts two-column data (retention time in min, response intensity)
- **Canvas Visualization**: Plots response curve with interactive tooltips
- **Calculation Engine**: Finds min/max values, computes 20%-80% thresholds, calculates spatial resolution
- **Peak Detection**: Identifies the closest minimum value to the selected maximum based on user-specified peak position (left/right)

## Core Workflow

1. User pastes retention time and intensity data
2. Enters scan speed (μm/s) to convert time to distance
3. Clicks "绘制图表" to plot the curve
4. Selects peak position (maximum left or right of minimum)
5. Confirms min/max values (editable)
6. Clicks "确认并计算" to calculate spatial resolution
7. Result shows distance between 20% and 80% response points

## Key Variables

- `distances`: Array of distances in μm (calculated from retention time × 60 × scan speed)
- `intensities`: Array of response intensity values
- `highlightMaxIdx`, `highlightMinIdx`: Indices of the max/min points used for current calculation
- `peakPosition`: User selection ("right" or "left") determining which side to search for minimum

## Important Implementation Details

- **Min/Max Finding**: Uses user-input values to find corresponding indices, not global extrema
- **Intersection Calculation**: Only searches between the selected min and max indices to avoid multiple peaks
- **Canvas Coordinates**: Y-axis uses `canvas.height - padding - (value / maxInt) * height` formula
- **Highlight Updates**: Reset `highlightMaxIdx` and `highlightMinIdx` when plotting new data; update them in `confirmValues()`
- **Tooltip**: Shows distance (μm) and intensity on hover; click to copy intensity to clipboard

## Deployment

Deployed on Vercel. GitHub repository: `https://github.com/finezhuxy-hub/MSI-spatial-resolution-calculator`

Push changes to trigger automatic redeployment:
```bash
git add index.html && git commit -m "message" && git push
```

## Common Modifications

- **Add new calculation metrics**: Extend `confirmValues()` and result display section
- **Change visualization**: Modify `draw()` and `drawWithLines()` functions
- **Adjust UI layout**: Edit CSS in `<style>` section
- **Modify tooltip behavior**: Update mouse event listeners in script section
