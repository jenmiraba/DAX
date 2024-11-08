# Dynamic DAX Welcome Message Formula 👋

## Overview
This DAX formula creates a personalized welcome message that dynamically changes based on the time of day, displaying the current user's name, date, and appropriate greeting with themed emojis. It's a perfect addition to any Power BI dashboard that requires a personal touch.

## Case Studies & Business Use Cases 🎯

1. **Corporate Dashboards** 💼
   - Personalized analytics dashboards for employees
   - Internal reporting systems with user recognition
   - Team performance dashboards with personal greetings

2. **Customer Service Portals** 👥
   - Customer-facing dashboards with personalized welcome
   - Support ticket systems with time-appropriate greetings
   - Client reporting interfaces

3. **Educational Platforms** 📚
   - Student performance dashboards
   - Teacher administrative panels
   - Educational progress tracking systems

## Generic Code

```dax
Welcome Message = 
VAR _CurrentDate = FORMAT(TODAY(), "dd \o\f mmmm \o\f yyyy", "en-US")

VAR _CurrentHour = HOUR(NOW())

VAR _Greeting = 
    SWITCH(
        TRUE(),
        _CurrentHour >= 0 && _CurrentHour < 6, "Good night",
        _CurrentHour >= 6 && _CurrentHour < 12, "Good morning",
        _CurrentHour >= 12 && _CurrentHour < 20, "Good afternoon",
        _CurrentHour >= 20 && _CurrentHour < 24, "Good night"
    )

VAR _TimeEmoji = 
    SWITCH(
        TRUE(),
        _CurrentHour >= 0 && _CurrentHour < 6, "💤",    
        _CurrentHour >= 6 && _CurrentHour < 12, "☕",   
        _CurrentHour >= 12 && _CurrentHour < 20, "🌅",   
        _CurrentHour >= 20 && _CurrentHour < 24, "🌜" 
    )

VAR _AtPosition = SEARCH("@", USERPRINCIPALNAME(), 1, LEN(USERPRINCIPALNAME()))

VAR _BaseName = IF(
        _AtPosition > 0,
        LEFT(USERPRINCIPALNAME(), _AtPosition - 1),
        USERPRINCIPALNAME()
    )

VAR _FormattedName = UPPER(LEFT(_BaseName, 1)) & LOWER(MID(_BaseName, 2, LEN(_BaseName) - 1))

VAR _WeekDay = FORMAT(TODAY(), "dddd", "en-US")
VAR _CapitalizedWeekDay = UPPER(LEFT(_WeekDay, 1)) & LOWER(MID(_WeekDay, 2, LEN(_WeekDay) - 1))

RETURN
_Greeting & ", " & _FormattedName & " " & _TimeEmoji & 
UNICHAR(10) & "Today is " & _CapitalizedWeekDay & "," & UNICHAR(10) & _CurrentDate
```

## Notes

1. To customize the formula for your needs:
   - Adjust the time ranges in `_CurrentHour` conditions to match your business hours
   - Modify the greeting text in `_Greeting` to match your preferred language
   - Change the emojis in `_TimeEmoji` to match your brand style
   - Update the date format in `_CurrentDate` to match your regional preferences

2. Language customization:
   - Change the "en-US" locale to your preferred language locale
   - Update the greeting messages accordingly
   - Modify the date format string to match your language conventions

3. User name formatting:
   - The formula extracts the username from before the "@" symbol
   - It automatically capitalizes the first letter and lowercase the rest
   - Modify the `_AtPosition` logic if your username format differs

4. The formula supports Unicode characters and emojis through:
   - Direct emoji insertion in the `_TimeEmoji` variable
   - `UNICHAR(10)` for line breaks
   - Proper case formatting for names and weekdays

---

🌟 Inspired by [Abiola David's LinkedIn post](https://www.linkedin.com/posts/abioladavid01_powerbi-dax-activity-7242990517976731648-lmlu?)
