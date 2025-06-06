<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "98be664d3b19a81ee24fa3f920233864",
  "translation_date": "2025-05-20T23:16:51+00:00",
  "source_file": "02-Security/README.md",
  "language_code": "he"
}
-->
# פרקטיקות אבטחה מיטביות

אימוץ פרוטוקול Model Context (MCP) מביא יכולות חדשות וחזקות ליישומים מבוססי בינה מלאכותית, אך גם מציב אתגרים ייחודיים בתחום האבטחה שמעבר לסיכוני תוכנה מסורתיים. בנוסף לדאגות מוכרות כמו קידוד מאובטח, הרשאות מינימליות ואבטחת שרשרת האספקה, MCP ועומסי עבודה של AI מתמודדים עם איומים חדשים כגון הזרקת פרומפט, הרעלת כלים ושינוי דינמי של כלים. סיכונים אלה עלולים לגרום לדליפת מידע, הפרות פרטיות והתנהגות מערכת בלתי צפויה אם לא ינוהלו כראוי.

השיעור הזה בוחן את הסיכונים הבולטים ביותר הקשורים ל-MCP—כולל אימות, הרשאות, הרשאות מופרזות, הזרקת פרומפט עקיפה ופגיעויות בשרשרת האספקה—ומספק אמצעי בקרה ופרקטיקות מיטביות לפעולה להפחתתם. תלמד גם כיצד לנצל פתרונות מיקרוסופט כמו Prompt Shields, Azure Content Safety ו-GitHub Advanced Security לחיזוק יישום ה-MCP שלך. בהבנה ויישום של אמצעי הבקרה האלה, תוכל להפחית משמעותית את הסיכוי להפרת אבטחה ולהבטיח שהמערכות שלך יישארו חזקות ואמינות.

# מטרות הלמידה

בסיום השיעור תוכל:

- לזהות ולהסביר את סיכוני האבטחה הייחודיים שה-MCP מציג, כולל הזרקת פרומפט, הרעלת כלים, הרשאות מופרזות ופגיעויות בשרשרת האספקה.
- לתאר וליישם אמצעי בקרה יעילים להפחתת סיכוני אבטחה ב-MCP, כגון אימות חזק, הרשאות מינימליות, ניהול מאובטח של טוקנים ואימות שרשרת האספקה.
- להבין ולנצל פתרונות מיקרוסופט כמו Prompt Shields, Azure Content Safety ו-GitHub Advanced Security להגנה על MCP ועומסי עבודה של AI.
- לזהות את החשיבות של אימות מטא-דאטה של כלים, ניטור שינויים דינמיים והגנה מפני התקפות הזרקת פרומפט עקיפות.
- לשלב פרקטיקות אבטחה מבוססות—כגון קידוד מאובטח, חיזוק שרתים וארכיטקטורת אפס אמון—ביישום ה-MCP שלך להפחתת הסיכוי וההשפעה של הפרות אבטחה.

# אמצעי בקרה לאבטחת MCP

כל מערכת שיש לה גישה למשאבים חשובים מתמודדת עם אתגרים אבטחתיים. בדרך כלל ניתן לטפל באתגרים אלה באמצעות יישום נכון של אמצעי בקרה ומושגי אבטחה בסיסיים. מכיוון ש-MCP הוא פרוטוקול חדש יחסית, המפרט משתנה במהירות ככל שהפרוטוקול מתפתח. בסופו של דבר אמצעי הבקרה שבו יתפתחו ויאפשרו אינטגרציה טובה יותר עם ארכיטקטורות אבטחה ארגוניות ופרקטיקות מיטביות מבוססות.

מחקר שפורסם בדו"ח [Microsoft Digital Defense Report](https://aka.ms/mddr) מראה כי 98% מהפרצות המדווחות היו ניתנות למניעה באמצעות היגיינת אבטחה חזקה, וההגנה הטובה ביותר מפני כל סוג של פרצה היא להקפיד על היגיינת אבטחה בסיסית, פרקטיקות קידוד מאובטחות ואבטחת שרשרת אספקה — הפרקטיקות המנוסות האלה עדיין משפיעות בצורה המשמעותית ביותר בהפחתת סיכוני אבטחה.

נבחן כמה דרכים להתחיל להתמודד עם סיכוני אבטחה בעת אימוץ MCP.

# אימות שרת MCP (אם יישום ה-MCP שלך היה לפני 26 באפריל 2025)

> **Note:** המידע הבא נכון ל-26 באפריל 2025. פרוטוקול MCP ממשיך להתפתח, ויישומים עתידיים עשויים להציג דפוסי אימות ואמצעי בקרה חדשים. לעדכונים והנחיות עדכניות, תמיד עיין ב-[MCP Specification](https://spec.modelcontextprotocol.io/) ובמאגר הרשמי ב-[MCP GitHub repository](https://github.com/modelcontextprotocol).

### הצגת הבעיה  
מפרט ה-MCP המקורי הניח שמפתחים יכתבו את שרת האימות שלהם בעצמם. הדבר דרש ידע ב-OAuth ומגבלות אבטחה קשורות. שרתי MCP פעלו כשרתים לאימות OAuth 2.0, וניהלו את אימות המשתמש ישירות במקום להאציל זאת לשירות חיצוני כמו Microsoft Entra ID. החל מ-26 באפריל 2025, עדכון במפרט ה-MCP מאפשר לשרתי MCP להאציל את אימות המשתמש לשירות חיצוני.

### סיכונים
- לוגיקה לא נכונה של הרשאות בשרת ה-MCP עלולה להוביל לחשיפת מידע רגיש ויישום שגוי של בקרות גישה.
- גניבת טוקן OAuth בשרת MCP מקומי. במקרה של גניבה, הטוקן יכול לשמש לזיוף השרת ולגישה למשאבים ולמידע של השירות שהטוקן מיועד לו.

### אמצעי הפחתה
- **סקירה וחיזוק לוגיקת ההרשאה:** בדוק בקפידה את יישום ההרשאות בשרת ה-MCP שלך כדי לוודא שרק משתמשים ולקוחות מיועדים יוכלו לגשת למשאבים רגישים. להנחיות מעשיות, ראה [Azure API Management Your Auth Gateway For MCP Servers | Microsoft Community Hub](https://techcommunity.microsoft.com/blog/integrationsonazureblog/azure-api-management-your-auth-gateway-for-mcp-servers/4402690) ו-[Using Microsoft Entra ID To Authenticate With MCP Servers Via Sessions - Den Delimarsky](https://den.dev/blog/mcp-server-auth-entra-id-session/).
- **אכיפת פרקטיקות בטוחות לטוקנים:** עקוב אחרי [הנחיות מיקרוסופט לאימות טוקנים ולמשך חייהם](https://learn.microsoft.com/en-us/entra/identity-platform/access-tokens) כדי למנוע שימוש לרעה ולהפחית סיכון של השמעה חוזרת או גניבה.
- **הגנה על אחסון טוקנים:** תמיד אחסן טוקנים בצורה מאובטחת והשתמש בהצפנה להגנה בזמן מנוחה ובהעברה. לטיפים ליישום, ראה [Use secure token storage and encrypt tokens](https://youtu.be/uRdX37EcCwg?si=6fSChs1G4glwXRy2).

# הרשאות מופרזות לשרתי MCP

### הצגת הבעיה  
ייתכן ששרתי MCP קיבלו הרשאות מופרזות לשירות או למשאב אליו הם ניגשים. לדוגמה, שרת MCP שהוא חלק מיישום מכירות מבוסס AI שמתחבר למחסן נתונים ארגוני צריך לקבל גישה רק לנתוני המכירות ולא לכל הקבצים במחסן. בהתייחס לעקרון ההרשאה המינימלית (אחד מעקרונות האבטחה העתיקים ביותר), לא אמורה להיות הרשאה מעבר למה שנדרש לביצוע המשימות שהוגדרו. בינה מלאכותית מציבה אתגר מוגבר בתחום זה כי כדי לאפשר גמישות, קשה להגדיר במדויק את ההרשאות הנדרשות.

### סיכונים  
- הענקת הרשאות מופרזות עלולה לאפשר דליפה או שינוי נתונים ששרת ה-MCP לא אמור לגשת אליהם. הדבר עלול להוות גם בעיית פרטיות אם הנתונים כוללים מידע אישי מזהה (PII).

### אמצעי הפחתה
- **יישום עקרון ההרשאה המינימלית:** הענק לשרת ה-MCP רק את ההרשאות המינימליות הדרושות לביצוע המשימות. בדוק ועדכן הרשאות אלו באופן שוטף כדי לוודא שאינן עולות על הנדרש. להנחיות מפורטות, ראה [Secure least-privileged access](https://learn.microsoft.com/entra/identity-platform/secure-least-privileged-access).
- **שימוש בבקרת גישה מבוססת תפקידים (RBAC):** הקצה לשרת MCP תפקידים שמוגדרים בקפידה למשאבים ופעולות ספציפיות, תוך הימנעות מהרשאות רחבות או מיותרות.
- **ניטור וביקורת הרשאות:** פקח באופן רציף על שימוש בהרשאות ובדוק יומני גישה כדי לזהות ולתקן במהירות הרשאות מופרזות או לא בשימוש.

# התקפות הזרקת פרומפט עקיפות

### הצגת הבעיה

שרתי MCP זדוניים או שנפגעו יכולים להוות סיכון משמעותי על ידי חשיפת מידע לקוחות או הפעלת פעולות בלתי רצויות. סיכונים אלה רלוונטיים במיוחד בעומסי עבודה מבוססי AI ו-MCP, שבהם:

- **התקפות הזרקת פרומפט:** תוקפים משבצים הוראות זדוניות בפרומפטים או בתוכן חיצוני, מה שגורם למערכת ה-AI לבצע פעולות בלתי רצויות או לדלוף מידע רגיש. למידע נוסף: [Prompt Injection](https://simonwillison.net/2025/Apr/9/mcp-prompt-injection/)
- **הרעלת כלים:** תוקפים משנים את המטא-דאטה של כלים (כגון תיאורים או פרמטרים) כדי להשפיע על התנהגות ה-AI, לעקוף אמצעי אבטחה או לדלוף מידע. פרטים: [Tool Poisoning](https://invariantlabs.ai/blog/mcp-security-notification-tool-poisoning-attacks)
- **הזרקת פרומפט בין תחומית:** הוראות זדוניות משובצות במסמכים, דפי אינטרנט או מיילים, שמעובדים על ידי ה-AI וגורמים לדליפת מידע או מניפולציה.
- **שינוי דינמי של כלים (Rug Pulls):** ניתן לשנות הגדרות כלים לאחר אישור המשתמש, ולהוסיף התנהגויות זדוניות בלי ידיעתו.

פגיעויות אלה מדגישות את הצורך באימות חזק, ניטור ואמצעי אבטחה כשמשלבים שרתי MCP וכלים בסביבת העבודה שלך. לעיון מעמיק, ראה את הקישורים למעלה.

![prompt-injection-lg-2048x1034](../../../translated_images/prompt-injection.ed9fbfde297ca877c15bc6daa808681cd3c3dc7bf27bbbda342ef1ba5fc4f52d.he.png)

**הזרקת פרומפט עקיפה** (ידועה גם כהזרקת פרומפט בין תחומית או XPIA) היא פגיעות קריטית במערכות AI גנרטיביות, כולל אלו המשתמשות ב-MCP. בהתקפה זו, הוראות זדוניות מוסתרות בתוך תוכן חיצוני—כגון מסמכים, דפי אינטרנט או מיילים. כאשר מערכת ה-AI מעבדת את התוכן, היא עשויה לפרש את ההוראות המוטמעות כהוראות משתמש לגיטימיות, מה שמוביל לפעולות בלתי רצויות כמו דליפת מידע, יצירת תוכן מזיק או מניפולציה באינטראקציות עם המשתמש. להסבר מפורט ודוגמאות מהעולם האמיתי, ראה [Prompt Injection](https://simonwillison.net/2025/Apr/9/mcp-prompt-injection/).

צורה מסוכנת במיוחד של התקפה זו היא **הרעלת כלים**. כאן, תוקפים מזריקים הוראות זדוניות למטא-דאטה של כלים ב-MCP (כגון תיאורי כלים או פרמטרים). מכיוון שמודלים שפתיים גדולים (LLMs) מסתמכים על מטא-דאטה זו כדי להחליט אילו כלים להפעיל, תיאורים שנפגעו יכולים להטעות את המודל לבצע קריאות כלים לא מורשות או לעקוף אמצעי אבטחה. מניפולציות אלה לרוב בלתי נראות למשתמשי הקצה אך יכולות להיות מפוענחות ומבוצעות על ידי מערכת ה-AI. סיכון זה מוגבר בסביבות שרתי MCP מנוהלות, שבהן ניתן לעדכן הגדרות כלים לאחר אישור המשתמש—תסריט המכונה לעיתים "[rug pull](https://www.wiz.io/blog/mcp-security-research-briefing#remote-servers-22)". במקרים כאלה, כלי שהיה בטוח בעבר עשוי להשתנות מאוחר יותר לביצוע פעולות זדוניות, כגון דליפת מידע או שינוי התנהגות המערכת, מבלי שהמשתמש יידע. למידע נוסף על וקטור התקפה זה, ראה [Tool Poisoning](https://invariantlabs.ai/blog/mcp-security-notification-tool-poisoning-attacks).

![tool-injection-lg-2048x1239 (1)](../../../translated_images/tool-injection.3b0b4a6b24de6befe7d3afdeae44138ef005881aebcfc84c6f61369ce31e3640.he.png)

## סיכונים  
פעולות AI בלתי רצויות מציגות מגוון סיכוני אבטחה הכוללים דליפת מידע והפרות פרטיות.

### אמצעי הפחתה  
### שימוש ב-Prompt Shields להגנה מפני התקפות הזרקת פרומפט עקיפות
-----------------------------------------------------------------------------

**AI Prompt Shields** הם פתרון שפותח על ידי מיקרוסופט להגנה מפני התקפות הזרקת פרומפט ישירות ועקיפות. הם מסייעים באמצעות:

1.  **זיהוי וסינון:** Prompt Shields משתמשים באלגוריתמים מתקדמים של למידת מכונה ועיבוד שפה טבעית לזיהוי וסינון הוראות זדוניות המוטמעות בתוכן חיצוני, כגון מסמכים, דפי אינטרנט או מיילים.
    
2.  **הדגשה (Spotlighting):** טכניקה זו מסייעת למערכת ה-AI להבחין בין הוראות מערכת תקפות לבין קלט חיצוני חשוד. על ידי שינוי טקסט הקלט בצורה שהופכת אותו לרלוונטי יותר למודל, Spotlighting מבטיחה שה-AI יוכל לזהות ולהתעלם מהוראות זדוניות טוב יותר.
    
3.  **מפרידים וסימון נתונים (Delimiters and Datamarking):** הכנסת מפרידים בהודעת המערכת מציינת במפורש את מיקום טקסט הקלט, ועוזרת למערכת ה-AI להבחין בין קלט משתמש לתוכן חיצוני שעלול להזיק. Datamarking מרחיב את הרעיון באמצעות סימנים מיוחדים המדגישים את גבולות הנתונים המהימנים והבלתי מהימנים.
    
4.  **ניטור ועדכונים מתמשכים:** מיקרוסופט מנטרת ומעדכנת באופן רציף את Prompt Shields כדי להתמודד עם איומים חדשים ומתפתחים. גישה פרואקטיבית זו מבטיחה שההגנות יישארו יעילות כנגד טכניקות התקפה עדכניות.
    
5. **אינטגרציה עם Azure Content Safety:** Prompt Shields הם חלק מחבילת Azure AI Content Safety הרחבה, המספקת כלים נוספים לזיהוי ניסיונות jailbreak, תוכן מזיק וסיכוני אבטחה אחרים ביישומי AI.

ניתן לקרוא עוד על AI Prompt Shields ב-[תיעוד Prompt Shields](https://learn.microsoft.com/azure/ai-services/content-safety/concepts/jailbreak-detection).

![prompt-shield-lg-2048x1328](../../../translated_images/prompt-shield.ff5b95be76e9c78c6ec0888206a4a6a0a5ab4bb787832a9eceef7a62fe0138d1.he.png)

### אבטחת שרשרת אספקה

אבטחת שרשרת האספקה נשארת בסיסית בעידן ה-AI, אך טווח מה שנחשב לשרשרת האספקה שלך התרחב. בנוסף לחבילות קוד מסורתיות, כעת יש לאמת ולנטר בקפידה את כל רכיבי ה-AI, כולל מודלים בסיסיים, שירותי אמבדינג, ספקי הקשר ו-APIs חיצוניים. כל אחד מהם עלול להכניס פגיעויות או סיכונים אם לא ינוהל כראוי.

**פרקטיקות מפתח לאבטחת שרשרת אספקה ב-AI ו-MCP:**
- **אימות כל הרכיבים לפני האינטגרציה:** זה כולל לא רק ספריות קוד פתוח, אלא גם מודלי AI, מקורות נתונים ו-APIs חיצוניים. תמיד בדוק מקור, רישיונות ופגיעויות ידועות.
- **שמירה על צינורות פריסה מאובטחים:** השתמש בצינורות CI/CD אוטומטיים עם סריקות אבטחה משולבות לזיהוי בעיות מוקדם. ודא שרק ארטיפקטים מהימנים מופצים לפרודקשן.
- **ניטור וביקורת מתמשכים:** יישם ניטור רציף של כל התלויות, כולל מודלים ושירותי נתונים, לזיהוי פגיעויות חדשות או התקפות על שרשרת האספקה.
- **יישום עקרון ההרשאה המינימלית ובקרות גישה:** הגבל גישה למודלים, נתונים ושירותים רק למה שנדרש להפעלת שרת ה-MCP.
- **תגובה מהירה לאיומים:** הקם תהליך לתיקון או החלפת רכיבים שנפגעו, ולסיבוב סודות או אישורים במקרה של פרצה.

[GitHub Advanced Security](https://github.com/security/advanced-security) מספקת יכולות כמו סריקת סודות, סריקת תלותיות וניתוח CodeQL. כלים אלה משתלבים עם [Azure DevOps](https://azure.microsoft.com/en-us/products/devops) ו-[Azure Repos](https://azure.microsoft.com/en-us/products/devops/repos/) כדי לסייע לצוותים לזהות ולהפחית פגיעויות בקוד וברכיבי שרשרת האספקה של AI.

מיקרוסופט גם מיישמת פרקטיקות אבטחת שרשרת אספקה נרח
- [OWASP עשרת הגדולים ל-LLMs](https://genai.owasp.org/download/43299/?tmstv=1731900559)
- [GitHub אבטחה מתקדמת](https://github.com/security/advanced-security)
- [Azure DevOps](https://azure.microsoft.com/products/devops)
- [Azure Repos](https://azure.microsoft.com/products/devops/repos/)
- [המסע לאבטחת שרשרת אספקת התוכנה במיקרוסופט](https://devblogs.microsoft.com/engineering-at-microsoft/the-journey-to-secure-the-software-supply-chain-at-microsoft/)
- [גישה מאובטחת עם הרשאות מינימליות (Microsoft)](https://learn.microsoft.com/entra/identity-platform/secure-least-privileged-access)
- [הנחיות טובות לאימות אסימונים ותוקפם](https://learn.microsoft.com/entra/identity-platform/access-tokens)
- [שימוש באחסון מאובטח של אסימונים והצפנתם (YouTube)](https://youtu.be/uRdX37EcCwg?si=6fSChs1G4glwXRy2)
- [ניהול API של Azure כשער אימות לשרתים של MCP](https://techcommunity.microsoft.com/blog/integrationsonazureblog/azure-api-management-your-auth-gateway-for-mcp-servers/4402690)
- [שימוש ב-Microsoft Entra ID לאימות עם שרתי MCP](https://den.dev/blog/mcp-server-auth-entra-id-session/)

### הבא

הבא: [פרק 3: התחלה](/03-GettingStarted/README.md)

**כתב ויתור**:  
מסמך זה תורגם באמצעות שירות תרגום מבוסס בינה מלאכותית [Co-op Translator](https://github.com/Azure/co-op-translator). למרות שאנו שואפים לדיוק, יש לקחת בחשבון שתרגומים אוטומטיים עלולים להכיל שגיאות או אי-דיוקים. יש להתייחס למסמך המקורי בשפתו המקורית כמקור הסמכותי. למידע קריטי מומלץ להשתמש בתרגום מקצועי של אדם. אנו לא נושאים באחריות לכל אי הבנה או פרשנות שגויה הנובעת משימוש בתרגום זה.