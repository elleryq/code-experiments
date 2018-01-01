---
layout: post
title: 'Part of Android CTS SimpleDateFormatTest in Jython'
date: 2013-12-31 08:12
comments: true
categories: 
---
同事問我為什麼會測試不過，就想說用 jython 試試看 PC 上的行為。
SimpleDateFormat("EEEE", Locale.ENGLISH).parseDate("Tuesday") 會得到 Tue Jan 06 00:00:00 CST 1970 ，用 UTC calendar 去取星期幾時，會得到 2 - Monday，而測試預期是得到 3 - Tuesday。
為什麼 Android 裡的測試會預期得到 3 呢?? 而 PC 上得到的結果是一樣的呢??

```
from java.text import SimpleDateFormat
from java.text import ParsePosition
from java.util import Locale
from java.util import TimeZone
from java.util import Calendar


def parseDate(locale, fmt, value):
    sdf = SimpleDateFormat(fmt, locale)
    pp = ParsePosition(0)
    d = sdf.parse(value, pp)
    if not d:
        return None
    c = Calendar.getInstance(TimeZone.getTimeZone("UTC"))
    c.setTime(d)
    return c

en = Locale.ENGLISH
assert Calendar.TUESDAY == parseDate(en,
                                     "EEEE",
                                     "Tuesday").get(Calendar.DAY_OF_WEEK)

```
