<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "706b9b075dc484b73a053e6e9c709b4b",
  "translation_date": "2025-05-25T13:27:43+00:00",
  "source_file": "04-PracticalImplementation/samples/python/README.md",
  "language_code": "fa"
}
-->
# پیاده‌سازی پایتون Model Context Protocol (MCP)

این مخزن شامل پیاده‌سازی پایتون پروتکل Model Context Protocol (MCP) است که نشان می‌دهد چگونه می‌توان یک برنامه سرور و کلاینت ایجاد کرد که با استفاده از استاندارد MCP با هم ارتباط برقرار می‌کنند.

## مرور کلی

پیاده‌سازی MCP شامل دو بخش اصلی است:

1. **MCP Server (`server.py`)** - سروری که ارائه می‌دهد:
   - **ابزارها**: توابعی که می‌توان از راه دور فراخوانی کرد
   - **منابع**: داده‌هایی که می‌توان دریافت کرد
   - **پرومپت‌ها**: قالب‌هایی برای تولید پرومپت برای مدل‌های زبانی

2. **MCP Client (`client.py`)** - برنامه کلاینتی که به سرور متصل می‌شود و از امکانات آن استفاده می‌کند

## ویژگی‌ها

این پیاده‌سازی چند ویژگی کلیدی MCP را نشان می‌دهد:

### ابزارها
- `completion` - تولید تکمیل متن از مدل‌های هوش مصنوعی (شبیه‌سازی شده)
- `add` - ماشین حساب ساده که دو عدد را جمع می‌کند

### منابع
- `models://` - بازگرداندن اطلاعات درباره مدل‌های هوش مصنوعی موجود
- `greeting://{name}` - بازگرداندن پیام خوش‌آمدگویی شخصی‌سازی شده برای یک نام مشخص

### پرومپت‌ها
- `review_code` - تولید پرومپتی برای بازبینی کد

## نصب

برای استفاده از این پیاده‌سازی MCP، بسته‌های مورد نیاز را نصب کنید:

```powershell
pip install mcp-server mcp-client
```

## اجرای سرور و کلاینت

### راه‌اندازی سرور

سرور را در یک پنجره ترمینال اجرا کنید:

```powershell
python server.py
```

همچنین می‌توان سرور را در حالت توسعه با استفاده از MCP CLI اجرا کرد:

```powershell
mcp dev server.py
```

یا در Claude Desktop نصب کرد (در صورت وجود):

```powershell
mcp install server.py
```

### اجرای کلاینت

کلاینت را در یک پنجره ترمینال دیگر اجرا کنید:

```powershell
python client.py
```

این کار به سرور متصل شده و همه ویژگی‌های موجود را نشان می‌دهد.

### استفاده از کلاینت

کلاینت (`client.py`) تمام قابلیت‌های MCP را نشان می‌دهد:

```powershell
python client.py
```

این کار به سرور متصل شده و همه ویژگی‌ها از جمله ابزارها، منابع و پرومپت‌ها را اجرا می‌کند. خروجی موارد زیر را نشان می‌دهد:

1. نتیجه ابزار ماشین حساب (۵ + ۷ = ۱۲)
2. پاسخ ابزار تکمیل به «معنای زندگی چیست؟»
3. فهرست مدل‌های هوش مصنوعی موجود
4. پیام خوش‌آمدگویی شخصی‌سازی شده برای «MCP Explorer»
5. قالب پرومپت بازبینی کد

## جزئیات پیاده‌سازی

سرور با استفاده از API `FastMCP` پیاده‌سازی شده است که انتزاع‌های سطح بالا برای تعریف سرویس‌های MCP فراهم می‌کند. در اینجا نمونه‌ای ساده‌شده از نحوه تعریف ابزارها آمده است:

```python
@mcp.tool()
def add(a: int, b: int) -> int:
    """Add two numbers together
    
    Args:
        a: First number
        b: Second number
    
    Returns:
        The sum of the two numbers
    """
    logger.info(f"Adding {a} and {b}")
    return a + b
```

کلاینت با استفاده از کتابخانه MCP client به سرور متصل شده و آن را فراخوانی می‌کند:

```python
async with stdio_client(server_params) as (reader, writer):
    async with ClientSession(reader, writer) as session:
        await session.initialize()
        result = await session.call_tool("add", arguments={"a": 5, "b": 7})
```

## اطلاعات بیشتر

برای اطلاعات بیشتر درباره MCP، مراجعه کنید به: https://modelcontextprotocol.io/

**سلب مسئولیت**:  
این سند با استفاده از سرویس ترجمه هوش مصنوعی [Co-op Translator](https://github.com/Azure/co-op-translator) ترجمه شده است. در حالی که ما در تلاش برای دقت هستیم، لطفاً توجه داشته باشید که ترجمه‌های خودکار ممکن است حاوی خطاها یا نواقصی باشند. سند اصلی به زبان بومی آن باید به عنوان منبع معتبر در نظر گرفته شود. برای اطلاعات حیاتی، ترجمه حرفه‌ای انسانی توصیه می‌شود. ما مسئول هیچ گونه سوءتفاهم یا برداشت نادرستی که از استفاده از این ترجمه ناشی شود، نیستیم.