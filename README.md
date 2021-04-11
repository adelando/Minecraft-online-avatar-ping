# Minecraft-online-avatar-ping
A website tool that allows admin and staff to show current online players as avatars

Here is a walkthrough to understand the code by Connor Simpson

https://stackoverflow.com/users/1993252/connor-simpson 


Starting off, by wrapping your code in $(document).ready this'll only run the code when the page has loaded.

$(document).ready(() => {
   // The document is ready, let's run some code!
});
Then add your AJAX request as normal inside this bracket.

$(document).ready(() => {

   let url = "https://api.minetools.eu/query/play.aydaacraft.online/25565";
   $.getJSON(url, response => {
     
   });

});
Okay, whilst writing this, I checked the URL provided by OP and saw that it was timing out so I've grabbed a sample response from the Minetools' documentation.

{
  "MaxPlayers": 200,
  "Motd": "A Minecraft Server",
  "Playerlist": [
     "Connor",
     "Kamil",
     "David"
  ],
  "Players": 3,
  "Plugins": [],
  "Software": "CraftBukkit on Bukkit 1.8.8-R0.2-SNAPSHOT",
  "Version": "1.8.8",
  "status": "OK"
}
So in your JSON response, you can see that Playerlist is a array which can contain multiple things in one variable. You can also iterate through an array, which is what we'll be doing to build the image URLs.

We iterate through an array using forEach.

$(document).ready(() => {

   let url = "https://api.minetools.eu/query/play.aydaacraft.online/25565";
   $.getJSON(url, response => {
      response.Playerlist.forEach(playerName => {
         console.log(playerName);
      });
   });

});
//Console:
//Connor
//Kamil
//David
Now that we're iterating through the player list we can start assembling the URLs for these images and adding them to your document's body.

I've cleaned up your HTML, take note of the new div#user-images I've added. This'll be the place where jQuery will add your images from the forEach loop.

<div class="card"> 
    <div class="icon">
        <img src="https://cdn.worldvectorlogo.com/logos/minecraft-1.svg">
    </div>
    <div class="header">
        <div class="image">
            <img src="https://res.cloudinary.com/lmn/image/upload/e_sharpen:100/f_auto,fl_lossy,q_auto/v1/gameskinnyc/u/n/t/untitled-a5150.jpg" alt="" />
        </div>

        <h2>Server Status</h2>
        
    </div>
    
    <!-- This div tag will need to hide when there is no error, or say when there is. -->
    <div id="rest">Loading...</div>
    
    <!-- The user images will be added inside this div. -->
    <div id="user-images"></div>
</div>
Now we have our HTML ready we can start using the jQuery function appendTo to add elements into our div#user-images.

$(document).ready(() => {

   let url = "https://api.minetools.eu/query/play.aydaacraft.online/25565";
   $.getJSON(url, response => {
      response.Playerlist.forEach(playerName => {
         $(`<img src="https://cravatar.eu/avatar/${playerName}" />`).appendTo("#user-images");
      });
   });

});
Your div#user-images should start filling up with the images of players from the Playerlist array.

I noticed you added a simple way of showing whether or not there's an error with the API. We can interact with div#rest to show/hide or change text depending on the success of the response.

$(document).ready(() => {

   let url = "https://api.minetools.eu/query/play.aydaacraft.online/25565";
   $.getJSON(url, response => {
      if(response.error){
         $("#rest").html("The server is offline!");
      }else{
         //There is no error, hide the div#rest
         $("#rest").hide();
    
         response.Playerlist.forEach(playerName => {
            $(`<img src="https://cravatar.eu/avatar/${playerName}" />`).append
