# CSharpLiveProject
2 week project completing stories for a Theatre company.

As my final project before completing my courses at The Tech Academy, this entailed utilizing multiple languages, concepts, and interactions between database tables and front-end facing tasks. Examples of some of the stories are listed below.

CREATING ENTITY MODEL
-Created a model for class called Rental that allowed items to be saved to a database.
-Added two additional classes that inherit from Rental (parent class)
-Scaffolded CRUD pages to interact with the controller for various methods (Create, Edit, Delete, etc)

```public ActionResult Create([Bind(Include = "RentalId,RentalName,RentalCost,FlawsAndDamages")] Rental rental, int? PurchasePrice, bool SuffocationHazard, bool ChokingHazard, int RoomNumber, int MaxOccupancy, int SquareFootage)
        { 
           
            //Logic for RentalEquipment
            if (PurchasePrice > 0)  //if PurchasePrice has value then we know we're dealing with RentalEquipment
            {
                RentalEquipment rentalequipment = new RentalEquipment();  //instantiate new instance of object rentalequipment    named 'rentalequipment'
                rentalequipment.RentalName = rental.RentalName; //explicitly telling program that the RentalName field is = to the current rentals.RentalName
                rentalequipment.RentalCost = rental.RentalCost;
                rentalequipment.FlawsAndDamages = rental.FlawsAndDamages;
                rentalequipment.PurchasePrice = Convert.ToInt32(PurchasePrice); //same as above but include which class object (rentalequipment instead of rental)
                rentalequipment.SuffocationHazard = Convert.ToBoolean(SuffocationHazard);
                rentalequipment.ChokingHazard = Convert.ToBoolean(ChokingHazard); //anytime its boolean or int use convert statement define this way

                db.Rentals.Add(rentalequipment); //add new object rentalequipment to database
                db.SaveChanges();                  //save to db
                return RedirectToAction("Index");  //send user to Index page once saved
}

```
STYLING CREATE AND EDIT PAGES
-Added placeholders to fields as well as styling to match desired color scheme
-Cleaned up formatting and general styling to fit requested aesthetics
![csharpscreenshot1](https://user-images.githubusercontent.com/107223231/201410291-eb32e93e-8097-44eb-b76c-d4c2c796a385.jpg)


ADDING CLASS SPECIFIC FIELDS TO VIEW FOR APPROPRIATE CLASS SELECTION
-Created a dropdown menu on Create/Edit pages that allow user to select one of three different class objects
-Based on user selection, page will adjust and display only the fields for item's respective class 
-Extraneous fields that are not related to the item's class are hidden upon selection
-Ensured the create and edit buttons were properly adding items or ammending them in database
-Accomplished tasks using a mix of Razor syntax, C#, CSS, and HTML

```
<!--RENTAL FORM-->
            <div class="rental-create--rentalformcomponent">
                <div class="form-group rental-create--formgroupheading1">
                    @Html.LabelFor(model => model.RentalName, "Rental Name", htmlAttributes: new { @class = "control-label col-md-2 rental-create--formlabel1" })
                    <!--above line "Rental Name" is the label adjusted heading/how to change that field-->
                    <div class="col-md-10">
                        @Html.EditorFor(model => model.RentalName, new { htmlAttributes = new { @class = "form-control rental-create--formcontrols", placeholder =    "Enter rental name" } })
                        @Html.ValidationMessageFor(model => model.RentalName, "", new { @class = "text-danger" })
                    </div>
                </div>
```

STYLING INDEX PAGE
-Removed previous table layout and replaced with a grid layout
-Created a layout that includes each item in database as a card in a row/column format
-Three sections are displayed each containing items of different classes
-Some of the items are types of equipment that may have hazards users need to be aware of, these are represented with eye catching pill badges where appropriate
-Dynamically displayed whether or not items had certain properties present or not
-Added placeholder images to each card to improve overall aesthetics

```
  @foreach (var item in Model)
    {
        if (item.GetType() == typeof(RentalEquipment))
        {
            var suffHazard = ((RentalEquipment)item).SuffocationHazard;
            var chokHazard = ((RentalEquipment)item).ChokingHazard;

        <!--untab below part leaving this way while working-->
        <div class="col rental-index--rentalcolumns">
            <div class="card rental-index--card1A">
                <div class="rental-index--innerphotocontainer1A">
                    <img src="~/Content/images/theater.jpg" alt="theatre photo" class="card-img-top rental-index--rentalsectionimage1" />
                    <br />
                </div>
                <p class="rental-index--rentalsectiontext1A">
                    @Html.DisplayFor(model => item.RentalName)
                    <br />$@Html.DisplayFor(model => item.RentalCost)/day
                    <br />

                    
                    @if (suffHazard == true)
                    {
                        <span class="badge badge-pill badge-warning">Suffocation Hazard</span>

                    }
                    @if (chokHazard == true)
                    {
                        <span class="badge badge-pill badge-warning">Choking Hazard</span>
                    }
```
            
DELETE AND DETAILS PAGE STYLING
-Added functionality to each page so that the properties specific to each database item's class are displayed and any fields that aren't relevant to that item are hidden
-Ensured the delete and edit buttons on the page properly functioned with database items
-Did basic styling/formatting to clean up overall look of each page

```
<!--RENTAL ROOM SPECIFIC-->
        @if (Model.GetType() == typeof(RentalRoom))
        {
            var squareFootageValue = ((RentalRoom)Model).SquareFootage;
            var roomOccupancy = ((RentalRoom)Model).MaxOccupancy;
            var roomNumber = ((RentalRoom)Model).RoomNumber;
            
            
            <dt>
                @Html.DisplayNameFor(model => RentalRoom.RoomNumber)
            </dt>

            <dd>
                @Html.DisplayFor(model => roomNumber)
            </dd>

            <dt>
                @Html.DisplayNameFor(model => RentalRoom.SquareFootage)
            </dt>

            <dd>
                @Html.DisplayFor(model => squareFootageValue)
            </dd>

            <dt>
                @Html.DisplayNameFor(model => RentalRoom.MaxOccupancy)
            </dt>

            <dd>
                @Html.DisplayFor(model => roomOccupancy)
            </dd>
        }
        ```
