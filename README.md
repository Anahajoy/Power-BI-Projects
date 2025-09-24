# Power-BI-Projects
The dashboard was inspired by the Workout Wednesday Challenge and showcases creative approaches to data storytelling, making insights clear, interactive, and actionable.

# data source link 
-- selected only from the **domain** - work&quality job, health, saftey,subjective well being
-- from the **sex** - female, male
-- from the **time period** range from 2010-2025


https://data-explorer.oecd.org/vis?fs[0]=Topic%2C1%7CSociety%23SOC%23%7CWell-being%20and%20beyond%20GDP%23SOC_WEL%23&pg=0&fc=Topic&bp=true&snb=26&vw=tb&df[ds]=dsDisseminateFinalDMZ&df[id]=DSD_HSL%40DF_HSL_CWB&df[ag]=OECD.WISE.WDP&df[vs]=1.1&dq=.11_2%2B11_1%2B9_3%2B9_2%2B8_2%2B8_1_DEP%2B7_2%2B7_1_DEP%2B6_2_DEP%2B6_2%2B5_3%2B5_1%2B4_3%2B4_1%2B3_2%2B3_1%2B2_7%2B2_2%2B2_1%2B1_3%2B1_2%2B1_1.._T._T._T.&lom=LASTNOBSERVATIONS&lo=1&pd=%2C&to[TIME_PERIOD]=false



# Progress bar measure
safteybar = 
VAR totalBlocks = 20               -- total rectangles
VAR per = [saftey%] * 100          -- percentage
VAR filledBlocks = ROUND(per / 100 * totalBlocks, 0)
VAR green = "%2349CF8F"
VAR yellow = "%23FE782E"
VAR red = "%23D9534F"
VAR yellowlimit = 20
VAR redlimit = 10

 --- the colour code based on condition 
VAR blockColor =
    SWITCH(
        TRUE(),
        per <= redlimit, red,
        per <= yellowlimit, yellow,
        per > yellowlimit, green
    )

-- generate SVG rectangles
VAR svgBlocks =
    CONCATENATEX(
        GENERATESERIES(1, totalBlocks, 1),
        "<rect x='" & (([Value]-1)*12) & "' y='0' width='10' height='20' fill='" &
        IF([Value] <= filledBlocks, blockColor, "%23e0e0e0") &
        "' rx='2' ry='2'/>",
        ""
    )

-- genefate the final bar
RETURN
    "data:image/svg+xml;utf8," &
    "<svg width='" & (totalBlocks*12) & "' height='20' xmlns='http://www.w3.org/2000/svg'>" &
    svgBlocks &
    "</svg>"



    
