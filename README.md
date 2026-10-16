

# Phishing Analysis Fundamentals - TryHackMe Walkthrough

## סקירת החדר
החדר מתמקד ביסודות ניתוח הודעות דוא"ל זדוניות (Phishing Analysis). במהלך התרגול נלמד ונחקור את מבנה הדוא"ל: כותרי הודעה (Headers), גוף ההודעה (Body), ניתוח קישורים וקבצים מצורפים, תוך הפקת אינדיקטורים לחדירה (IoCs) וזיהוי מקור התקיפה.

---

<img width="1753" height="511" alt="image" src="https://github.com/user-attachments/assets/551814ba-334a-4645-b216-449667e95768" />

# מדריך מעבדה: Phishing Analysis Fundamentals - TryHackMe Walkthrough

## סקירת החדר

החדר נועד להקנות הבנה עמוקה של רכיבי הודעות דואל וכיצד מנתחים דואל החשוד כפישינג או כזדוני. במהלך המעבדה נלמד כיצד הודעת דואל מועברת בין שרתים, מה המבנה של כתובת דואל, כיצד מנתחים כותרי הודעה (Headers), כיצד בוחנים את גוף ההודעה (Body) בקוד המקור, וכיצד משחזרים קבצים מצורפים מתוך קידוד Base64 לצורך בדיקה.

---

## משימה 1: Introduction (מבוא והקמת סביבת העבודה)

### מה לומדים במשימה זו?

משימה זו מציגה את האיומים הנפוצים ביותר בארגונים מודרניים: דואר זבל (Spam) והונאות דיוג (Phishing). מוסבר כיצד פעולה תמימה של משתמש אחד, כגון לחיצה על קישור זדוני או פתיחת קובץ מצורף, עלולה להוביל לחדירה ראשונית של תוקף לרשת הארגונית. תפקידו של אנליסט האבטחה הוא לבדוק הודעות גולמיות, להבין את מקורן האמיתי, ולקבוע אם ההודעה בטוחה או זדונית.

### שאלות ותשובות:

שאלה: I am ready to learn about phishing analysis!

* תשובה: יש ללחוץ על כפתור Complete לסיום המשימה.

---

## משימה 2: The Email Address (מבנה כתובת הדואל)

### מה לומדים במשימה זו?

המשימה עוסקת באנטומיה הבסיסית של כתובת דואל, שנכנסה לשימוש עוד בשנות ה-70 ברשת ARPANET על ידי ריי טומלינסון. הכתובת מורכבת משלושה חלקים מרכזיים:

1. Username: שם תיבת הדואר של המשתמש הספציפי בשרת היעד.
2. סימן @: תו המפריד בין שם המשתמש לבין שרת היעד.
3. Domain name: שם הדומיין המגדיר את שרת הדואל שאחראי לקבל את ההודעה.
ההבנה הזו חיונית כדי שאנליסט יזהה במהירות ניסיונות זיוף והתחזות לדומיינים לגיטימיים.

### שאלות ותשובות:

שאלה: Identify the domain used in the following email address: hatsalesman@tryhatme.com

* תשובה: tryhatme.com

---

## משימה 3: Email Delivery (מסלול העברת הדואל והפרוטוקולים)

### מה לומדים במשימה זו?

משימה זו מפרטת את אופן התנועה של הודעת דואל ברשת ואת הפרוטוקולים השונים המעורבים בתהליך:

* SMTP (Simple Mail Transfer Protocol): הפרוטוקול האחראי על שליחת הדואל מתוכנת הלקוח לשרת, ובין שרתים ברשת.
* DNS: שירות שמות המתחם שבאמצעותו שרת הדואל השולח מאתר את שרת היעד (באמצעות רשומות MX).
* POP3: פרוטוקול המוריד את ההודעות מהשרת להתקן יחיד ומוחק אותן בדרך כלל מהשרת, מה שמונע סנכרון בין מכשירים.
* IMAP: פרוטוקול השומר את ההודעות על גבי השרת ומסנכרן את תיבת הדואר בין מספר מכשירים במקביל (כמו טלפון נייד ומחשב).

### שאלות ותשובות:

שאלה 1: Which protocol is responsible for sending an email from a client to a mail server?

* תשובה: SMTP

שאלה 2: Which service is used to look up the recipient domain’s mail server?

* תשובה: DNS

שאלה 3: Bob wants to access his email from multiple devices, including his phone and laptop. Which protocol should he use?

* תשובה: IMAP

---

## משימה 4: Email Headers (ניתוח כותרי הודעה)

### מה לומדים במשימה זו?

המשימה מתמקדת בחלק החשוב ביותר בחקירת פישינג: כותרי הדואל (Headers). כותרים אלו מכילים מטא-דאטה מקיפה על ההודעה, כגון מי השולח, מי הנמען, תאריך השליחה, שרתי הביניים שדרכם עברה ההודעה, וכתובת ה-IP האמיתית שממנה יצאה ההודעה. מוסבר כיצד לפתוח את מקור ההודעה הגולמי (Message Source) בתוכנת לקוח כמו Thunderbird באמצעות צירוף המקשים Ctrl + U כדי לחשוף שדות טכניים מוסתרים כמו X-Originating-Ip.

### שאלות ותשובות:

שאלה 1: What is the full subject line of email1.eml?

* תשובה: Help protect your budget by protecting your home

שאלה 2: View the message source of email1.eml using Thunderbird in your VM. What the IP address listed as the X-Originating-Ip?

* תשובה: 43.255.56.161

---

## משימה 5: Email Body (גוף ההודעה ושחזור קבצים מצורפים)

### מה לומדים במשימה זו?

משימה זו מלמדת כיצד לחקור את גוף ההודעה (Body), שיכול להיות טקסט פשוט או קוד HTML מלא. בדיקת קוד ה-HTML מאפשרת לאתר קישורים מוסתרים, תמונות חיצוניות ורכיבים זדוניים.
בנוסף, נלמד כיצד קבצים מצורפים (Attachments) מוטמעים בתוך ההודעה:

* Content-Type: מציין את סוג הקובץ (למשל application/pdf).
* Content-Disposition: מציין שמדובר בקובץ מצורף ומכיל את שמו המקורי (filename).
* Content-Transfer-Encoding: מציג את שיטת הקידוד, בדרך כלל base64.
כאשר קובץ מצורף מוטמע כטקסט Base64, אנליסט יכול לחלץ את מחרוזת הטקסט, לפענח אותה באמצעות כלים כמו CyberChef, ולשחזר את הקובץ המקורי לצורך ניתוח מעמיק.

### שאלות ותשובות:

שאלה 1: Open up the email2.txt file to view the source of an attachment. What is the Content-Type of the attachment?

* תשובה: application/pdf

שאלה 2: What is the name of the attachment from the previous question?

* תשובה: zmqpalgh.pdf

שאלה 3: Decode the base64 string using either a PDF converter or CyberChef. What is the hidden flag value?

* תשובה: THM{BENIGN_PDF_ATTACHMENT}

---

## משימה 6: Types of Phishing (סוגי התקפות פישינג וניתוח הודעה חשודה)

### מה לומדים במשימה זו?

משימה זו מלמדת על הווקטורים והשיטות השונות שבהן תוקפים מנצלים את מערכת הדואל כדי להטעות משתמשים:

* Spam ו-Malspam: דואר זבל המוני, כאשר malspam כולל גם נוזקות.
* Phishing: התחזות לגורם מוכר כדי לדלות פרטים אישיים, סיסמאות או מידע פיננסי.
* Spear Phishing: הודעת דיוג ממוקדת ומותאמת אישית לאדם או לארגון ספציפי לאחר איסוף מידע מוקדם.
* Whaling: תת-סוג של Spear Phishing שמכוון לבכירים ביותר בארגון (מנכל, סמנכל כספים).
* Smishing: פישינג באמצעות הודעות טקסט (SMS) לטלפונים ניידים.
* Vishing: הנדסה חברתית ופישינג המבוצעים באמצעות שיחות טלפון קוליות.

כמו כן, נלמדים הסימנים המזהים הנפוצים ביותר בהודעת פישינג: זיוף שורת השולח (Spoofed From), יצירת תחושת דחיפות ומלכודות זמן, חיקוי מותג מוכר (לוגואים וצבעים), שגיאות ניסוח או שפה לא טבעית, פנייה כללית חסרת שם, קישורים מוסתרים או מקוצרים, וקבצים מצורפים מתחזים.

מושג קריטי שנלמד הוא Defanging (נטרול כתובות וקישורים): הפיכת קישורים, דומיינים וכתובות IP לבלתי לחיצים (למשל הפיכת נקודה לתוך סוגריים מרובעים `[.]`) כדי למנוע לחיצה בשוגג במהלך חקירת אבטחה וכדי למנוע חסימה של הדו"ח על ידי מערכות סינון.

### שאלות ותשובות:

שאלה 1: Which reputable organization is being spoofed in this phishing attempt?

* תשובה: Home depot

שאלה 2: What is the sender’s email address?

* תשובה: support@teckbe.com

שאלה 3: Inspect the email message source. What is the defanged X-Originating-IP?

* תשובה: 103[.]234[.]236[.]83

שאלה 4: Continue analyzing the email message source. Which mail server generated the Authentication-Results header?

* תשובה: atlas102.free.mail.gq1.yahoo.com

---

## משימה 7: Conclusion (סיכום ומתקפות BEC)

### מה לומדים במשימה זו?

משימת הסיכום מאגדת את כל שלבי החקירה שתרגלנו: החל מאנטומיית כתובת הדואל, דרך מסלול המעבר בין השרתים, בדיקת כותרים וגוף ההודעה ברמת קוד המקור, ועד שחזור קבצים מצורפים וזיהוי מאפייני הונאה.
בנוסף מוצג המושג Business Email Compromise (ובקיצור BEC) - מתקפה מתוחכמת שבה תוקף משיג גישה לחשבון דואל ארגוני לגיטימי אמיתי ומשתמש בו כדי להונות עובדים אחרים, לאשר העברות כספים כוזבות או לגנוב מידע פנימי תוך ניצול יחסי האמון הפנים-ארגוניים.

### שאלות ותשובות:

שאלה: What attack, signified by the acronym BEC, uses a compromised email to trick employees into fraud?

* תשובה: Business Email Compromise

---
