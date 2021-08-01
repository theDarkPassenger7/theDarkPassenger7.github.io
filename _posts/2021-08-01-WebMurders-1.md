---
layout: post
title: "#01 - SQLI: The First Murder Is Never Forgotten"
---

![Dex](/images/dexYoung.jpeg)

*June 7 - 1994 - My First Murder* <br> <br>
*Erik Horte*, pedophile. He abused 13 boys/girls between the ages of 7 and 13 years. The "Justice" sentenced *Erik Horte* to 3 years of house arrest in a protected palace. 13 broken lives. 13 destinies marked. True justice will come soon Erik. Wait for me. I'm coming.

I remember my first murder like it was yesterday. My face was pale. Sweat dripped from my forehead. A mixture of adrenaline held my hand. I've been waiting for this moment for 17 years. For 17 years, *The Dark Passenger* had been hiding inside me. *The Dark Passenger* was ready to go out. The blood was boiling in my head. My heartbeat increased by the hour, minute by minute, second by second. That day, I chose to use a classic weapon. I love the SQLi blade and it allows you to fully savor the moment. The Blade SQLi has many variations of use but few know its true effectiveness. *The Dark Passenger* is ready to show you.

*Erik Horte* was in a secure/protected Palace. The Palace consisted of 6 security Rooms.
My mission was to kill *Erik Horte on the night of June 7, 1994*. I was and am a nocturnal hunting animal. And you were my first prey.

I will tell you about my first murder, with a faithful reconstruction of all 6 Rooms, through *PHP/MYSQL Laboratories/Simulations* created by myself. The simulation will use a Database called *kill_Diary*, shown below:

![DB](/images/1Homicide/db.png)

The *DB kill_Diary* containing various tables, including the *murders* table. The *murders* table contains the names of the desired past/future victims: <br> <br>

![DB](/images/1Homicide/DB2.png) <br> <br>


Are you ready to meet *The Dark Passenger*? <br> <br>

![DB](/images/1Homicide/dexyoung.png) <br> <br>

### Dexter Morgan First Murder Kit:
- *SQLi Basic*
- *SQLi Multi/Stacked Query*
- *SQLi Evade Sanitizing/Escapes Functions Server Side*
- *SQLi Evade Sanitizing/Escapes Functions Client Side*
- *SQLi Evade Through Misconfigurations*
- *SQLi Evade Prepared Statements*
- *Slice Of Life*

![MurderKit](/images/murdkit.jpeg)

### SQLI Basic

The 1rd Room did not implement a security system.
Getting into the 1st room was child's play. Easier than a laugh for the Joker.

The 1rd Room takes the name of the possible past/future victim as Input and gives 1 if the victim has been killed or 0, if not yet killed. The Query was built as shown:

![DextLab2](/images/1Homicide/dextLab2.png) <br> <br>


How to bypass the 1rd Room? The Query can be easily bypassed as desired:

![DextLab3](/images/1Homicide/dextLab3.png) <br> <br>

*The Chamber of Secrets* has been opened. Will the *Basilisk* be waiting for me in the next Room?
I hope not, I don't own *Godric Gryffindor's sword* :)

![HPRif1](/images/1Homicide/chamber.jpeg) <br> <br>

*For more details, ask your best friend Google about "SQLi Union Attacks" and "Fields and Structure of a Database MYSQL"*

### SQLi Multi/Stacked Query
Fortunately, there was no *Basilisk* in the 2rd room. I would have had trouble dealing with the *Basilisk* without the *Sword of Godric Gryffindor*. In  2rd Room, in addition to not being implemented any security mechanism and building the Query as in the previous case, the system uses a *Database Connector/Driver* that supports *Multi Query*.

![DextLab4dext](/images/1Homicide/dextLab4.png)

![DextLab5dext](/images/1Homicide/dextLab5.png)

The 2 room is a paradise. It's like the *Batcave* for *Bruce Wayne* :3

![BatmanRif1](/images/1Homicide/batcave.jpeg)

*For more details, ask your best friend Google about "Is Multi Query Support implemented on the Client or Server side? Does Multi Query Support depend on Application or Server Side? What is the difference between Buffered Query and Unbuffered Query?"*

### SQLi Evade Sanitizing/Escapes Functions Server Side

I had arrived at the 3rd Room. A disturbing Room. The walls engraved with multiple Chinese characters. Poster on the walls with the word GBK.

The 3rd Room implemented a security system, in order to escape some characters, such as the quotation mark, using the *addslashes()*, *mysql_escape_string()*, and *mysql_real_escape_string()* functions, as shown in the figure:

![DextLab6dext](/images/1Homicide/dextLab6.png)

It is no longer possible to bypass the 3rd Room using the previous attack methods:

![DextLabEx1](/images/1Homicide/dexEx1.png)

The 3rd Room employs the use of an *Encodindg Multi Byte Charset GBK* through a directive configured on the Server side. The directive informs the Server that all information sent from the Client Side is GBK Encoded.

The goal was to manipulate Url/Request, in order to create a character recognized by the *Charset Encoding GBK*, and to evade/cut the presence of the quotation mark:

![DextLabEx2](/images/1Homicide/dexEx2.png)


In *Percent Encoding (UTF8 Hex)*, the string %27 equals to the quotation mark. The implemented escape function will take care of inserting the backslash character, %5C, a position prior to the quotation mark, obtaining: %5C%27. In GBK Charset, is present a Multi Byte character %87%5C (HEX).

Inserting the %87 character in Percent Encoding (UTF8 HEX) in the url allowed me to successfully Bypass the 3rd Room:

![DextLabEx3](/images/1Homicide/dexEx3.png)


![DextLabEx4](/images/1Homicide/dexEx4.png)


In order to clearly show evasion/cutting of the quote character, the *Charset GBK* was set in the header of the *Server HTTP Response*:

![DextLabEx5](/images/1Homicide/dexEx5.png)

And as *Sloth* would say, *Supersloooth*!

![DextLabEx6](/images/1Homicide/goonies.jpg)

*For more details, ask your best friend Google about "How really works Encoding? What is URL Percent Encoding? What are the types of Encoding? What is Internal and External encoding? What is the Character_set_client Variable and how does it work? Are there any other vulnerable Encoding Charset? Is UTF8 vulnerable?*

### SQLi Evade Sanitizing/Escapes Functions Client Side


I had arrived at the 4rd Chamber. The need to kill increased room after room. Still 3 Rooms and The Dark Passenger will be satisfied.

The 4rd Room allowed to choose the desired *Encoding Charset*, by executing the "SET NAME CHARSET" Query, under the curtains. The "SET NAME CHARSET" Query, directly on the Client Side, allows you to set/modify the type of Encoding used/expected for the data sent from the client side to the Server for the current connection, as shown in the figure:

 ![DextLab12dext](/images/1Homicide/dextLab12.png)

 Also, the 4rd Room, like the previous Room, implements the safety mechanism that escapes some characters, such as the quotation mark, using the *addslashes()*, *mysql_escape_string()*, and *mysql_real_escape_string()* functions, as shown in the figure:


 ![DextLab13dext](/images/1Homicide/dextLab13.png)

I selected GBK and rerun the attack as in the previous case:

![DextLab14dext](/images/1Homicide/dextLab14.png)

What would have happened if the "mysql_set_charset" directive is used instead of "SET NAMES CHARSET in conjuction with mysql_real_escape_string?

The "mysql_set_charset" directive, in addition to behaving like the "SET NAMES CHARSET" Query, except for differences, such as for example, the setting of various variables, has the task of saving the Encoding Charset selected on the Application/Client side. The "mysql_real_escape_string" function gets the previous saved information and escapes special characters in a string for use in an SQL statement, taking into account the current charset of the connection.

In this case, bypass would not have been possible. I would have had to find another way and maybe use different weapons.



![DextLab16dext](/images/1Homicide/dextLab16.png)
![DextLab17dext](/images/1Homicide/dextLab17.png)


The 4rd Room was as easy as catching a fly for Miyagi :3

![rifKarKid](/images/1Homicide/myagi.jpeg)


*For more details, ask your best friend Google about: "What's the difference between "SET NAMES CHARSET" and "mysql_set_charset". How does the "mysql_set_charset" directive work? Which variables are set by the "mysql_set_charset" directive and what role do they have?*

### SQLi Evade Through Misconfigurations ###

I had arrived at room 5 and the blade was getting hotter and warmer. The 5rd Room was divided into 2 different sections, one after the other.

The 1 section of the 5 Room, implements the safety mechanism that escapes some characters, such as the quotation mark, using the *addslashes()*, *mysql_escape_string()* functions but I realized it was enabled the "NO_BACKSLASH_ESCAPES" directive. The *"NO_BACKSLASH_ESCAPES"* directive disable the default use of the backslash character as an escape character.

Enabling the *"NO_BACKSLASH_ESCAPES"* directive can makes it useless to use the escape functions *addslashes()* and *mysql_escape_string()*. The Bypass of section 1 of Room 5 is quick and easy, as shown in the figure:

![DextLab21dext](/images/1Homicide/dextLab21.png)

The 2nd section of the 5 Room is identical to the previous section, but uses the *mysql_real_escape_string()* function as an escape function and builds the query as shown in the figure:

![DextLab24dext](/images/1Homicide/DextLab24.png)


Again, the Bypass was quick and easy:

![DextLab25dext](/images/1Homicide/dextLab25.png)

What would have happened if the "mysql_real_escape" function had been used and the query had been constructed in the following way, as shown in the figura? The "mysql_real_enscape_string" function escapes the quotation mark, not by inserting the backslash character but by inserting a new quotation mark:

![DextLab22dext](/images/1Homicide/dextLab22.png)


Although the "NO_BACKSLASH_ESCAPES" directive can open up many avenues of attack, it is also a Workaround to the GBK Encoding Charset problem! Since the backslash character is not inserted, the formation of the previously seen character in Encoding Multi Byte GBK is no longer possible. How to Bypass? I should have identified other weaknesses, and probably helped myself with other weapons.

If I were room 5, I'd feel like Oswald Cobblepot :(

![RifGoth1](/images/1Homicide/oswald.jpeg)

*For more details, ask your best friend Google about "What is the "NO_BACKSLASH_ESCAPES" directive and why is it a Workaround for the GBK Charset Encoding issue?"*


### SQLi Evade Prepared Statements ###

I felt like a child who had just stuck a pen into the flesh of a bully. Certainly not a normal child, but a happy and satisfied child. Here I felt so. Happy and ready to sink the blade into the belly of a person who did not deserve to live. I had arrived at the last and 6th room. The smell of murder was near.


The 6rd room, like the preceding one, has been divided into two sub-sections.

The first section involved the use of Prepared Statements and the build of the Query as shown in the figure. The prepared statements combine the variable with the compiled SQL statement, this means that the SQL and variables are sent separately and the variables are interpreted simply as strings, not as part of the SQL statement. For a moment I feared the worst, then I noticed that the Prepared Statement was written without binding the variables:

![DextLab18dext](/images/1Homicide/dextLab18.png)




The 2nd section of the 6 Room, built the Query, as shown in the figure:

![DextLab19dext](/images/1Homicide/dextLab19.png)

The 2nd section of the 6 Room used Encoding Charset GBK and used the Prepared Queries including the Binding of the variables, but they were "Emulated Prepared Statements". A Misconfiguration that will lead to your death Erik.

Since the Emulated Prepared Statements usually deal with the escape of special characters, such as the quotation mark, the bypass was performed using the techniques seen above, using Bypass Encoding GBK:

![DextLab20dext](/images/1Homicide/dexEx4.png)

*You killed the dreams of those who lived on cartoons. Now you will be the protagonist of a cartoon.*

![RifScoob](/images/1Homicide/scooby.jpeg)

*For more details, ask your best friend Google about "What are "Prepared Statements"? Are they enabled by default? If so, in which cases?"*

### Slice Of Life ###

I had prepared the murder room in detail. The beast, naked, tied to a cot, in the center of the room. Around, on the wall, stick photos of all his victims. The last thing he will see before he dies is the 13 broken lives and dreams.

![DextLab30dext](/images/1Homicide/dexKill.jpeg)

The Dark Passenger has been fed.

![DextLab31dext](/images/1Homicide/dexblood.jpeg)
