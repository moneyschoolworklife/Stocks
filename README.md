# Stocks
Stocks
//@version=5
indicator("Custom Technical Analysis Tool", overlay=true)

// Input parameters for customization
fvgLength = input.int(3, title="FVG Length", minval=1)
projectionLength = input.int(5, title="Projection Length", minval=1)
edgarSensitivity = input.float(1.5, title="EDGAR Sensitivity", minval=0.1)
alertEnabled = input.bool(true, title="Enable Alerts")

// Fair Value Gap (FVG) Calculation
fvgUp = ta.highest(high, fvgLength) < ta.lowest(low, fvgLength)
fvgDown = ta.lowest(low, fvgLength) > ta.highest(high, fvgLength)

// Plot FVG
plotshape(series=fvgUp, location=location.belowbar, color=color.new(color.green, 50), style=shape.labelup, text="FVG Up")
plotshape(series=fvgDown, location=location.abovebar, color=color.new(color.red, 50), style=shape.labeldown, text="FVG Down")

// Projection Levels Calculation
projectionHigh = ta.highest(high, projectionLength)
projectionLow = ta.lowest(low, projectionLength)

// Plot Projection Levels
plot(projectionHigh, color=color.blue, linewidth=2, title="Projection High")
plot(projectionLow, color=color.orange, linewidth=2, title="Projection Low")

// EDGAR (Equilibrium Dynamics and Gap Analysis for Reversals) Calculation
edgarThreshold = ta.atr(14) * edgarSensitivity
edgarReversalUp = low < ta.lowest(low, 14) - edgarThreshold
edgarReversalDown = high > ta.highest(high, 14) + edgarThreshold

// Plot EDGAR Reversals
plotshape(series=edgarReversalUp, location=location.belowbar, color=color.new(color.green, 50), style=shape.labelup, text="EDGAR Up")
plotshape(series=edgarReversalDown, location=location.abovebar, color=color.new(color.red, 50), style=shape.labeldown, text="EDGAR Down")

// Alerts
if (alertEnabled)
    alertcondition(fvgUp, title="FVG Up Alert", message="Fair Value Gap Up detected!")
    alertcondition(fvgDown, title="FVG Down Alert", message="Fair Value Gap Down detected!")
    alertcondition(edgarReversalUp, title="EDGAR Up Alert", message="EDGAR Reversal Up detected!")
    alertcondition(edgarReversalDown, title="EDGAR Down Alert", message="EDGAR Reversal Down detected!")

// Annotations
if (fvgUp)
    label.new(bar_index, low, text="FVG Up", color=color.new(color.green, 90), textcolor=color.white, style=label.style_label_down, yloc=yloc.belowbar)
if (fvgDown)
    label.new(bar_index, high, text="FVG Down", color=color.new(color.red, 90), textcolor=color.white, style=label.style_label_up, yloc=yloc.abovebar)
if (edgarReversalUp)
    label.new(bar_index, low, text="EDGAR Up", color=color.new(color.green, 90), textcolor=color.white, style=label.style_label_down, yloc=yloc.belowbar)
if (edgarReversalDown)
    label.new(bar_index, high, text="EDGAR Down", color=color.new(color.red, 90), textcolor=color.white, style=label.style_label_up, yloc=yloc.
