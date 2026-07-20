# Test Plan: Profile Tab (Android App)

Checklist based on real experience testing a mobile app. Screen names and logic are adapted to a fictional app called "AutoCare" (a car-service aggregator) to avoid disclosing employer data.

Platform: Android
Feature: updated Profile tab — guest mode, authorized user, settings

## 1. Guest Mode

Placeholder text is shown: "You're in guest mode now. To unlock all features, please log in"  
"Log in / Register" button (orange) — text is not cut off, alignment is correct  
"Continue with Google" button works  
Marketing banner displays correctly in guest mode  
Settings entry point is hidden/inaccessible for guest users  
App remains usable in guest mode (no crashes, no blocking screens)  

## 2. Header — Authorized User Profile

Avatar is displayed (or a placeholder if no photo is set)  
Full name and email are displayed  
Static numeric User ID is displayed below the profile block (non-editable)  
"Edit personal information" link is tappable and opens the correct screen  

## 3. Layout & System Logic

Widget order top to bottom matches design (Garage → Orders → Promotional Banner → Wishlist → Rewards → Delivery Address → Support)  
On screens with 640px height — elements compress proportionally, headers and text do not overlap  
Skeleton loader (grey placeholder) appears on tab load — each widget retries independently  
On load failure (timeout >10s) — inline error: 'Could not load [section]. Tap to retry'  
Other widgets remain accessible when one fails to load  
Page scrolls smoothly, bottom nav does not overlap any content  
Badge counters in widget headers match the actual count (vehicles, orders, wishlist)  
Spacing between blocks matches the design  
Privacy policy link is tappable  

## 4. Settings

Name and Email fields are locked (read-only)  
User ID is displayed as a static field  
All navigation links are present: Bank Details, Subscriptions, Deposit Account, Change Password, Current Country, System Preferences, Privacy Policy, Rate the App  

Personal Information sub-screen:
First Name and Last Name fields are editable  
Email is read-only  
Save button (checkmark) in the toolbar becomes active only once at least one field has been modified  
Tapping the avatar opens a bottom sheet: "How do you want to change your profile picture?" with options: Select from photos / Take a new photo / Delete photo / Cancel  
Adding a photo from gallery works  
Taking a photo with the camera works  
Uploaded photo is cropped to a circle at 56×56px without stretching or distortion  
Uploading a file that is not JPG/PNG or larger than 5MB triggers an error  
"Delete photo" resets the avatar to the default placeholder  
After returning from profile editing, the main screen renders correctly (no border/layout issues)  

Change Password:
It is a standalone screen (not embedded in Personal Info)  
Password input field placeholder has correct indentation  
Password change block is displayed correctly for guest users  

Current Country:
Current country is shown with a flag icon  
Tapping it opens the Address Management screen  

## 5. Rate the App

Selecting 4-5 stars redirects to Google Play  
Selecting 1-3 stars opens the device email client with the support address pre-filled  
With no selection — block stays idle, no action triggered  

## 6. Garage Widget

Badge counter:
Badge in the header = number of saved vehicles, hidden when 0 vehicles  

Empty state:
Add vehicle form is displayed inline on the dashboard  
Motivational header: "Add your car for perfect parts, easy maintenance, and no breakdowns"  

Search by registration number:
Country flag in the search field matches the user's current country (from Settings)  
Search button is active while typing  
Successful search → confirmation screen with pre-filled make/model/modification  
Failure → inline message: "Vehicle not found. Try manual entry instead"  

Manual Entry:
Three sequential steps: Car maker → Car model → Car modification — each opens a full-screen selector with a search field  
Each subsequent row is locked until the previous one is selected  
Changing car maker automatically resets Model and Modification  
"Add to garage" button is active (orange) only when all three fields are filled  

Duplicate detection:
Attempting to add an existing vehicle (same VIN or same make+model+year+modification) — inline: "This vehicle is already in your garage", submission is blocked  

Filled state (1-20 vehicles):
Cards in a single horizontal row with horizontal scrolling  
At exactly 20 vehicles, the "+" (Add a new car) button completely disappears from the widget header  
If the button is still visible and the user tries to add a 21st vehicle — tooltip: "You've reached the maximum number of vehicles"  

Vehicle card:
Vehicle photo (if set by user) or {Make} {Model} default image  
Mileage with "km" suffix  
Last service date  
Maintenance status tag: red (overdue jobs), yellow/amber (upcoming jobs), green (all clear)  
Tapping a card opens the Vehicle Details Hub  

"Add a new car" button (when vehicles exist):
Button is visible in the top-right corner of the Garage block header (until limit is reached)  
Tapping it opens the Add Vehicle screen  
Adding a vehicle from the catalog or app header is reflected synchronously in the Dashboard Garage block  

## 7. Orders Widget

Badge counter:
Badge = number of active orders (status: Delivered, Cancelled counted as inactive), hidden when 0 active orders  

Empty state:
Text: "You haven't placed any orders yet"  
"Start your shopping now" link navigates to the catalogue  

Filled state — Layout A (1 article in package):
Row 1: package status badge (left) + EDD (right), format "Est. Delivery: 14.04.2026"  
Row 2: item photo (square thumbnail, left) + item name (right, truncated to 2 lines)  

Filled state — Layout B (2+ articles in package):
Row 1: package status badge + EDD  
Row 2: horizontal row of item thumbnails — if they don't fit, the last visible slot is replaced by a "+N" overflow badge  
EDD is displayed correctly  
"+N" overflow badge is displayed correctly  

Package photo:
Displays the photo of the most expensive item in that specific package  

Return logic:
When an active return exists, the reclamation card completely overrides the regular order card  
Status badge changes to "Return in progress" or "Refund issued"  
When both a reclamation and an active order exist simultaneously, the reclamation takes priority on the dashboard, the order is still accessible via "see all >"  

Navigation:
Tapping a package card opens the Order Details page  
"see all >" opens the full Order History list  
Order history is correctly reset after logout  

Delivery status badge padding:
Internal padding matches the design  

## 8. Promotional Banner

Banner is displayed strictly between the Orders and Wishlist blocks  
Only one banner is shown at a time (the one with the highest priority in the CMS)  
If no active campaigns exist, the banner block is not rendered — no empty space is left  
