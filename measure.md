# 📏 DAX Measures
This document outlines all the key DAX measures used in the Power BI dashboard to derive insights for Atliq Grands.

### Revenue
Total revenue generated from customer bookings.

`Revenue = sum(fact_bookings[revenue_realized] )`

### Total Bookings
Count of all bookings made by customers, irrespective of status.

`Total Bookings = COUNT(fact_bookings[booking_id])`

### Total Capacity
Total number of room-nights available for sale across properties and dates.

`Total Capacity = SUM(fact_aggregated_bookings[capacity])`

### Total Successful Bookings
Total number of rooms that were successfully booked (and not cancelled).

`Total Successful Bookings = SUM(fact_aggregated_bookings[successful_bookings])`

### Occupancy %
Proportion of successfully booked rooms to total room capacity.

`Occupancy % = divide([Total Successful Bookings], [Total Capacity],0)`

### Average Rating
Average customer rating given to the hotels after their stay.

`Average Rating = AVERAGE(fact_bookings[ratings_given])`

### No of days
Total distinct number of dates present in the dataset.

`No of days = DATEDIFF(MIN(dim_date[date]), MAX(dim_date[date]), DAY) + 1`

### Total cancelled bookings
Number of bookings with status marked as "Cancelled".

`Total cancelled bookings = CALCULATE([Total Bookings], fact_bookings[booking_status]="Cancelled")`

### Cancellation %
Percentage of bookings that were cancelled out of total bookings.

`Cancellation % = DIVIDE([Total cancelled bookings], [Total Bookings])`

### Total Checked Out
Bookings where customers actually stayed at the hotel.

`Total Checked Out = CALCULATE([Total Bookings], fact_bookings[booking_status]="Checked Out")`

### Total no show bookings
Bookings that were not cancelled but customers didn’t show up.

`Total no shows bookings = CALCULATE([Total Bookings], fact_bookings[booking_status]="No Show")`

### No Show rate %
Proportion of no-show bookings out of all confirmed bookings.

`No Show rate % = DIVIDE([Total no shows bookings], [Total Bookings])`

### Booking % by Platform
Share of total bookings by each booking platform.

`Booking % by platform = DIVIDE([Total Bookings], CALCULATE([Total Bookings], ALL(fact_bookings[booking_platform])))`

### Booking % by Room class
Share of total bookings by each room class (Standard, Elite, Premium, etc.)

`Booking % by Room Class = DIVIDE([Total Bookings], CALCULATE([Total Bookings], ALL(dim_rooms[room_class])))`

### ADR 
Average revenue earned per room sold.

`ADR = DIVIDE( [Revenue], [Total Bookings],0)`

### Realisation %
Proportion of utilized bookings (Checked Out) to total bookings made (BRN).

`Realisation % = 1 - [No Show rate %] - [Cancellation %]`

### RevPAR
Average revenue per available room-night.

`RevPAR = DIVIDE([Revenue], [Total Capacity], 0)`

### DBRN
Number of rooms booked per day.

`DBRN = DIVIDE( [Total Bookings], [No of days], 0)`

### DSRN 
Number of rooms available to be sold each day.

`DSRN = DIVIDE([Total Capacity], [No of days], 0)`

### DURN
Number of rooms actually utilized each day.

`DURN = DIVIDE([Total Checked Out], [No of days], 0)`

### Revenue WoW change %
Percentage change in revenue compared to the previous week.

`Revenue WoW change % = 
var selv = if(HASONEFILTER(dim_date[wn]), SELECTEDVALUE(dim_date[wn]), MAX(dim_date[wn]))
var revcw = CALCULATE([Revenue], dim_date[wn] = selv)
var revpw = CALCULATE([Revenue], FILTER(ALL(dim_date), dim_date[wn] = selv - 1))
return 
DIVIDE(revcw, revpw, 0) - 1`

### Occupancy WoW change %
Percentage change in room occupancy rate compared to the previous week.

`Occupancy WoW change % = 
var lsc = IF(HASONEFILTER(dim_date[wn]), SELECTEDVALUE(dim_date[wn]), MAX(dim_date[wn]))
var occw = CALCULATE([Occupancy %], dim_date[wn]= lsc)
var ocpw = CALCULATE([Occupancy %], FILTER(ALL(dim_date), dim_date[wn] = lsc - 1))
return 
DIVIDE(occw, ocpw, 0) - 1`

### ADR WoW change %
Percentage change in Average Daily Rate compared to the previous week.

`ADR WoW change % = 
var lsc = IF(HASONEFILTER(dim_date[wn]), SELECTEDVALUE(dim_date[wn]), MAX(dim_date[wn]))
var adcw = CALCULATE([ADR], dim_date[wn]= lsc)
var adpw = CALCULATE([ADR], FILTER(ALL(dim_date), dim_date[wn] = lsc - 1))
return 
DIVIDE(adcw, adpw, 0) - 1`

### RevPAR WoW change %
Percentage change in RevPAR compared to the previous week.

`RevPAR WoW change % = 
var lsc = IF(HASONEFILTER(dim_date[wn]), SELECTEDVALUE(dim_date[wn]), MAX(dim_date[wn]))
var recw = CALCULATE([RevPAR], dim_date[wn]= lsc)
var repw = CALCULATE([RevPAR], FILTER(ALL(dim_date), dim_date[wn] = lsc - 1))
return 
DIVIDE(recw, repw, 0) - 1`

### Realisation WoW change %
Percentage change in Realisation compared to the previous week.

`Realisation WoW change % = 
var lsc = IF(HASONEFILTER(dim_date[wn]), SELECTEDVALUE(dim_date[wn]), MAX(dim_date[wn]))
var recw = CALCULATE([Realisation %], dim_date[wn]= lsc)
var repw = CALCULATE([Realisation %], FILTER(ALL(dim_date), dim_date[wn] = lsc - 1))
return 
DIVIDE(recw, repw, 0) - 1`

### DSRN WoW change %
Percentage change in DSRN compared to the previous week.

`DSRN WoW change % = 
Var selv = IF(HASONEFILTER(dim_date[wn]),SELECTEDVALUE(dim_date[wn]),MAX(dim_date[wn]))
var dscw = CALCULATE([DSRN],dim_date[wn]= selv)
var dspw =  CALCULATE([DSRN],FILTER(ALL(dim_date),dim_date[wn]= selv-1))
return
DIVIDE(dscw,dspw,0)-1`
