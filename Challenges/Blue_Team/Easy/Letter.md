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

