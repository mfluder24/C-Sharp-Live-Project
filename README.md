# Live Project
# Introduction
During a two-week sprint, I was tasked to work on a C# live project, alongside several other students through Azure DevOps, to create an interactive website for a theatre/acting company. The project was built using ASP.NET MVC and Entity Framework.It was designed to be a CMS (content management service) for non-technical users to easily manage the display of their website, as well as manage subscriber login capabability, and maintain a wiki. It was my second time working with  a large previously established codebase. Creating assigned sections of the website included several tasks that were new to me, which invited me to learn some new skills and techniques while continuing to practise those I had already learnt during my bootcamp. We utilised a code first approach, and I worked on both front and back end tasks, including building models following a schema and working with CRUD functionality. I used Agile and Scrum methodology, with daily stand up meetings and regular virtual communication with my team and management.

Below are descriptions of the stories I worked on, as well as code snippets and screenshots of my finished portions of the website.

# INDEX
* [Donation Page](#donation-page)
* [Rental Item Model and CRUD Functionality](#rental-item-model-and-crud-functionality)
* [Rental Model and CRUD Functionality](#rental-model-and-crud-functionality)  
* [Skills Acquired](#skills-acquired)


# Donation Page
I began by cloning the existing project codebase onto my computer via Visual Studio 2019, Git, and Azure DevOps. I then used the Package Manager Console to restore NuGet Packages and get the database set up. After everything was functional, my first task was to create and style a simple Donation Page for the website. The page had several input fields that allowed users to enter and submit their information in a form. I utilised Bootstrap 4's grid layout to create a basic, responsive form that met the client's requested input fields, and used basic CSS to keep the page in the client's colour scheme.  

![Alt Text](https://i.imgur.com/qJ4hN7m.gif)

* [Index](#index)

# Rental Item Model and CRUD Functionality
In my following assigned story, I began work on creating the Entity Model called RentalItem. I created a RentalItem class, following a supplied schema, and added the DbSet to the context to save it to the database. From there, I created the RentalItems Controller and scaffolded the CRUD pages.

* **Create & Edit Pages:**  
After they had been scaffolded and rendered operational, I began to do some front end work on both the Create and Edit pages to make them more visually appealing, accessible, and appropriately themed to match the rest of the website. This involved utilising Bootstrap 4, Razor, HTML, and CSS to create functional and beautiful forms in the colour theme provided by the client.  
![Alt Text](https://i.imgur.com/27Q83t1.gif)  
![Alt Text](https://i.imgur.com/vGY9JyL.gif)

* **Photo Storage & Retrieval:**  
When the RentalItem model was created, it included a byte array used for converting and storing an item photo. I was tasked with creating a method to convert an uploaded photo to a byte[], save that byte[] to the database, and finally retrieve the byte[] from the db and convert it back to a photo for display on the website.
```
[HttpPost] // Converts photo to byte array
        public byte[] AddPhoto(HttpPostedFileBase photoFile)
        {
            byte[] bytes;
            using(BinaryReader br = new BinaryReader(photoFile.InputStream))
            {
                bytes = br.ReadBytes(photoFile.ContentLength);
            }
            return bytes;
        }

        // Converts byte array to photo and returns to page
        public ActionResult RenderImage(int id)
        {
            RentalItem rentalItem = db.RentalItems.Find(id);
            byte[] bytes = rentalItem.ItemPhoto;
            return File(bytes, "image/jpeg");
        }
```
I also fixed an error thrown on the Edit page when attempting to update an item without uploading a new photo. The scaffolded Edit page did not have access to the original byte[]; I corrected this by saving the original byte[] as a variable via TempData, and adjusting the Edit method with if statements with parameters regarding if a new photo had been added or not.  

```
RentalItem origPhoto = TempData["item"] == null ? db.RentalItems.Find(rentalItem.RentalItemId) : (RentalItem)TempData["item"];

            if (ModelState.IsValid)
            {
                if (photoFile != null)
                {
                    var photo1 = AddPhoto(photoFile);
                    rentalItem.ItemPhoto = photo1;
                }

                if (photoFile == null && rentalItem.ItemPhoto == null && origPhoto.ItemPhoto != null)
                {
                    rentalItem.ItemPhoto = origPhoto.ItemPhoto;
                }

                db.Entry(rentalItem).State = EntityState.Modified;
                db.SaveChanges();
                return RedirectToAction("Index");
            }
            return View(rentalItem);
```  

* **Index Page:**  
I was tasked with more front end work on the Index page, and utilised Bootstrap to create cards for each RentalItem that displayed the uploaded photo, name, date checked out, and expected return date on the card. The Edit and Delete links were accessible by hovering over the item photo and clicking on the apprpriate Font Awesome icon. Clicking the card description would link the user to the full Details page of the individual item. Everything was created and styling using Bootstrap, Razor, HTML, and CSS.  
![Alt Text](https://i.imgur.com/glqLj7v.gif)  
I was also asked to create a functional search bar feature on the Index page, so that users could search for a particular item by Name, or a matching word/phrase in the item Description.
```
 @using (Html.BeginForm("Index", "RentalItems", FormMethod.Get))
    {
   
        @Html.TextBox("srcString")
        <input type="submit" value="Search" class="RentalItem-Index--CreateBtn cms-bg-secondary" />
    }
    
    ---
    
 public ActionResult Index(string srcString)
 {
   if(srcString == null) // If no entries in search bar, displays all items
   {
     return View(db.RentalItems.ToList());
   }
            
   else // Searches for user input matching item name or item description
   {
    return View(db.RentalItems.Where(i => i.Item.Contains(srcString) || i.ItemDescription.Contains(srcString)).ToList());
   }
            
}
    
```
![Alt Text](https://i.imgur.com/O4zB4Wb.gif)  

* **Details & Delete Pages:**  
Continuing to do some front end work, I applied some basic styling to both the Details and Delete pages to make them both visually appealing, easy to use, and in the colour theme requested by the client. I continued to make use of Bootstrap, Razor, HTML, CSS, and Font Awesome icons to style these pages. 

![Alt Text](https://i.imgur.com/6he0rsz.gif)   
![Alt Text](https://i.imgur.com/7YO7m6V.gif) 

* [Index](#index)


# Rental Model and CRUD Functionality 
In this story, I created a new Entity Model, Rental, which followed a schema and utilised inheritance. Referencing the schema, I first created the Rental class, which had two child classes called RentalEquipment and RentalRoom. I then added the DbSet to the context, updated the database, and scaffolded the CRUD pages.

* [Index](#index)  

# Skills Acquired  
* Increased experience with Azure DevOps, understanding of project flow, Agile and Scrum methodology, and working collaborateively with a team.
* Working through stories gave me a solid understanding of working on tasks assigned to me and meeting deadlines.
* My comfort with and understanding of working with Version Control also improved during this project. 
* This project definitely pushed me to research new skills, functions, and techniques, as I came across tasks that I was unfamiliar with. This gave me a opportunity to learn new approaches, use new tools, and increase my skill set. I was also able to reach out to management to see if I was on the right track, or if I needed to search for other methods to complete my tasks.
* I grew to have a greater appreciation for debugging and its many uses, as well as Developer Tools in Chrome.
* I feel my confidence with ASP.NET, MVC, and Entity Framework has increased greatly during this project. I greatly enjoy using C#, and this allowed me to combine it with HTML, Razor, and CSS to create a functional and powerful website.
  
* [Index](#index)
