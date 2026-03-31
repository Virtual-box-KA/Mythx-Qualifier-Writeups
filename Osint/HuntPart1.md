# OSINT Writeup – Hunt Part 1

## Challenge

Jai and C0mrade already know their DAA marks.  
Do you?

**Flag format:**  
MythX{(JaiScore(30) + C0mradeScore(100) × 121212)}

**Example:**  
MythX{455354}

---

## Step 1: Initial Recon (C0mrade)

I started by searching for **"c0mrade"** using Google dorking techniques, but no useful results were found.

To refine the search, I added **"kiet"** to the query:

```bash
"c0mrade" "kiet"
```
This led me to a profile of **Dev Pandey**, where multiple posts referenced a team named *c0mrade*. This strongly suggested that Dev Pandey was the person I was looking for.

---

## Step 2: Profiling Dev Pandey

After analyzing his LinkedIn profile, I gathered:
- He is a **CSE student**
- Belongs to **KIET**
- Batch: **2023–2027**

---

## Step 3: Advanced Dorking

I attempted another search:
```bash
"dev pandey" "kiet"
```
This didn’t yield direct results, but I discovered an old website:

old.kiet.edu

 
I then refined my search using:

```bash
site:old.kiet.edu "dev pandey" "kiet"
```

This led to a **PDF uploaded by the CSE department** containing a list of students from the 2023–2027 batch.

---

## Step 4: Missing Data & Brute Force

Unfortunately, the page containing Dev Pandey’s roll number was missing.

To proceed, I decided to brute-force the roll number.

I navigated to:

erp.aktu.ac.in


However, login required:
- Roll number
- Date of Birth

---

## Step 5: Finding Date of Birth

I searched for Dev Pandey on Instagram and found a profile with:
- Mutual connections
- A **date of birth in the bio**

---

## Step 6: Brute Forcing ERP

Using:
- Extracted **DOB**
- Roll numbers in range **54 to 98**

I tried combinations until:

✅ At roll number **98**, I successfully accessed a profile.

From there, I retrieved the **DAA result**, which was:

**C0mrade Score = 62**

---

## Step 7: Identifying Jai

Next, I moved to finding **Jai**.

- Searched LinkedIn and CTF-related platforms
- Found a profile on the **ByteHunters page**
- Identified **Ashutosh (Jai) Singh**

Observations:
- Frequently used the username **"jai"** in CTFs
- Confirmed he is my batchmate

---

## Step 8: Extracting Jai’s Marks

Since he was from my batch, I explored internal academic data.

After some manual investigation, I found:
- **DAA MSE-1 marks (4th semester)**

**Jai Score = 24**

---

## Step 9: Final Calculation

Using the formula:


Flag = (JaiScore + C0mradeScore × 121212)


Substituting values:


= (24 + 62 × 121212)
= 7515168


---

## Final Flag

```bash
MythX{7515168}
```
