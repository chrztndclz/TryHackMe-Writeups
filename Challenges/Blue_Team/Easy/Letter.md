## Title: Letter

#### Description: Can you help us find out more about this letter?

#### Difficulty: Easy

#### Type: Blue

Task 1: 

It's just another Monday morning on your mail delivery route when an unusual letter catches your eye. The envelope is battered, riddled with holes as if it's been through a storm. The address is barely legible, and your coworkers at the post office wave it off as a lost cause.
But something about it nags at you.

You carefully open the damp envelope. Inside, you find a faded newspaper clipping and a short handwritten note. The clipping is torn and water-damaged, with key sections missing. The note is personal, clearly not meant for your eyes, but the fragments you can read hint at a story buried in time.

Objective:  Use the clues provided in the zip file to uncover the full name and age of the person mentioned in the note.

Flag format: THM{Name_Surname_age}, only the first letter of the name and surname should be capitalised

Example: THM{Pierre-Henry_Lagaffe_23}

---

Task File: 

letter.png

<img width="574" height="428" alt="image" src="https://github.com/user-attachments/assets/1ef279a7-556a-4de4-9365-4e18ab34fc47" />

Newspaper_clipping.png

<img width="985" height="385" alt="image" src="https://github.com/user-attachments/assets/f05a42f2-1ee0-4b83-954e-83d349f9bae5" />


Note.txt

Mon cher Édouard,

Aujourd'hui, en rangeant le grenier chez mes grands-parents, je suis tombée sur cette vieille coupure de journal. Ton arrière-grand-père n'avait même pas l'âge de passer le permis quand il s'est distingué ce jour-là. Le benjamin de l'équipe, et certainement pas le moins courageux.

Il serait si fier de te voir sur l'eau à ton tour.

Avec toute mon affection,
Audette

**Note Translation:** 

My dear Édouard,

Today, while tidying up my grandparents' attic, I came across this old newspaper clipping. Your great-grandfather wasn't even old enough to get his driver's license when he distinguished himself that day. The youngest member of the team—and certainly not the least courageous.

He would be so proud to see you out on the water, too.

With all my love,
Audette

---

**Answer the questions below: **

**Question 1: What is the postal code of the delivery address on the envelope?**

**Route 1 – Decoding the Postal Barcode (Intended Solution)**

The envelope contains the following barcode:

..IIIII   I.II.I   II..II   III..I   .II.II

<img width="434" height="323" alt="image" src="https://github.com/user-attachments/assets/15f0a7d8-f0d4-4312-9af0-9f96875ea2d7" />

This pattern resembles a **French Postal Barcode**, where each combination of bars corresponds to a digit.

```
0	..||||
1	.|.|||
2	.||.||
3	.|||.|
4	|..|||
5	|.|.||
6	|.||.|
7	||..||
8	||.|.|
9	|||..|
```

Replacing each I with a vertical bar (|) gives and this produces:

```
..IIIII   	I.II.I    	II..II    	III..I    	.II.II
   0	         6	         7	        9	           2
```

French postal barcodes are read **from right to left**, resulting in:

``**29760**``

**Route 2 – Using the Newspaper Clue (Alternative Solution)**

<img width="975" height="260" alt="image" src="https://github.com/user-attachments/assets/868bce9d-c0f1-4cef-b745-fdc22083aa52" />

The partially obscured newspaper contains the headline:

Une catastrophe sur les côtes du Finistère

Deux bateaux de pêche en perdition et deux canots de sauvetage sombrent au large de Penmarc

**Translation:**

A disaster off the coast of Finistère.

Two fishing boats in distress and two lifeboats sink off **Penmarch.**

Searching for the postal code of Penmarch reveals:

Penmarch, Finistère, France → Postal Code: 29760

This matches the value obtained from decoding the barcode.

**Note:** This is just another route, you can just have a hint that the removed part of the newspaper is something important or useful and you can try if it’s connected to the challenge, somehow it is and through that we found the postal code itself. Otherwise, the more correct route is decrypting the code in the letter. 

Final Answer:
``29760``

---

**What is the flag?**

Format: THM{Name_Surname_age}

<img width="975" height="381" alt="image" src="https://github.com/user-attachments/assets/d17f48ac-ea89-463b-b516-a8ffd5f98c58" />

**Step 1 – Identify the Newspaper**
The newspaper shown in the challenge is L'Ouest-Éclair, a French regional daily newspaper published in Brittany.

**One visible headline reads:**
   Amundsen a-t-il atteint le pôle Nord ?
   
**Translation:**
   Has Amundsen reached the North Pole?

   This refers to the Norwegian explorer Roald Amundsen and his 1925 Arctic expedition in the month of May.
   
**Note:** Use image search, then go to wikipedia

<img width="764" height="339" alt="image" src="https://github.com/user-attachments/assets/35c19729-e81c-4dec-b9d6-71825290dd55" />

<img width="514" height="132" alt="image" src="https://github.com/user-attachments/assets/5b783008-b451-4e25-88a6-ffc336f131bf" />


Link: https://en.wikipedia.org/wiki/L%27Ouest-%C3%89clair

and at the very bottom we have an external link going to 

Link: https://gallica.bnf.fr/ark:/12148/cb32830550k/date

<img width="983" height="398" alt="image" src="https://github.com/user-attachments/assets/5783791d-b737-41fc-9560-934ad4280c62" />

Let’s go to the date **May 1925**, to search for this specific newspaper 

<img width="654" height="376" alt="image" src="https://github.com/user-attachments/assets/929546d8-fbda-4a4e-82db-9b9c7db6ca3d" />

**Step 2 – Locate the Correct Newspaper Issue**

Using the newspaper archives, search through editions from **May 1925** until finding the issue containing the **Amundsen headline.**

<img width="975" height="468" alt="image" src="https://github.com/user-attachments/assets/6e353c42-cf89-4729-987c-0455cead287d" />

The matching edition is:

**24 May 1925**

Link: https://gallica.bnf.fr/ark:/12148/bpt6k648015m/f1.image

<img width="975" height="472" alt="image" src="https://github.com/user-attachments/assets/ed5a634e-508c-48dd-98ad-8d7fd13ea889" />

This edition contains 12 pages, but we will only focus on the page 1.

**Step 3 – Connect the Postal Code to the Newspaper**

Earlier, we recovered the postal code:
   29760
   
Searching for this postal code shows that it belongs to:
   Penmarch, Finistère, Brittany, France
   
The newspaper article also mentions:
   A disaster off the coast of Finistère, Two fishing boats in distress and two lifeboats sink off Penmarch

This confirms that the article about the maritime disaster is relevant to the challenge.

<img width="253" height="258" alt="image" src="https://github.com/user-attachments/assets/856b93d7-a62f-4e16-8391-b4f1f82daca0" />

<img width="164" height="316" alt="image" src="https://github.com/user-attachments/assets/9ed6550b-4e95-42a0-aa7e-a3a17f426049" />

**Step 4 – Analyze the Letter**

```
My dear Édouard,

Today, while tidying up my grandparents' attic, I came across this old newspaper clipping. Your great-grandfather wasn't even old enough to get his driver's license when he distinguished himself that day. The youngest member of the team—and certainly not the least courageous.

He would be so proud to see you out on the water, too.

With all my love,
Audette
```

From this note, we can infer that:
1.	We are looking for Édouard's great-grandfather. 
2.	He participated in the maritime incident described in the article. 
3.	He was the youngest member of the team. 
4.	He was involved in a water-related activity (fishing, sailing, or rescue work). 
5.	The surname likely begins with G, since Édouard is referred to as Édouard G.

**Step 5 – Research the Disaster**

Search: who is youngest crew in the article A disaster off the coast of Finistère, Two fishing boats in distress and two lifeboats sink off Penmarch

Result: https://kbcpenmarch.franceserv.com/la-catastrophe-du-23-mai-1925-selon-la-presse-locale.html

Searching for information about the Penmarch disaster leads to a historical article describing the event and the individuals who were recognized for their bravery.

One of the recipients listed is:

**Gourlaouen Yves Marie, 15 years old, ship's boy.**

<img width="1017" height="288" alt="image" src="https://github.com/user-attachments/assets/abb2e4c2-7774-4d6d-94be-1c4dbc4507bf" />

**Translation:**

Second-class Arpent Medal. Gourlaouen Yves Marie*, 15 years old, ship's boy, registered at Le Guilvinec, No. 9397.


Final Answer:

Using the required format:

**THM{Yves-Marie_Gourlaouen_15}**

