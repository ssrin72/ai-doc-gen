# مولد مستندات هوش مصنوعی

ابزاری قدرتمند برای تولید خودکار مستندات کد که با استفاده از هوش مصنوعی، مخازن نرم‌افزاری شما را تجزیه و تحلیل کرده و مستندات کاملی بر اساس آن‌ها ایجاد می‌کند. این سیستم از معماری چندعامله بهره می‌برد تا تحلیل‌های تخصصی انجام داده و مستندات منسجم تولید کند.

## فهرست مطالب

- [ویژگی‌ها](#ویژگی‌ها)
- [نصب](#نصب)
- [شروع سریع](#شروع-سریع)
- [استفاده](#استفاده)
- [پیکربندی](#پیکربندی)
- [معماری](#معماری)
- [مجوز](#مجوز)

## ویژگی‌های کلیدی

- **Multi-Agent تحلیل**: عوامل هوشمند متخصص در تحلیل ساختار کد، جریان Data، Dependencies، Request Flow و API
- **تولید مستندات هوشمند**: ایجاد خودکار فایل‌های README کامل با بخش‌های قابل Customize
- **یکپارچه‌سازی با GitLab**: تحلیل خودکار پروژه‌های GitLab با قابلیت ایجاد Merge Request
- **پردازش موازی**: اجرای همزمان Analysis Agents برای Performance بهتر
- **پیکربندی انعطاف‌پذیر**: تنظیمات YAML با قابلیت بازنویسی متغیرهای محیط
- **سازگاری گسترده**: پشتیبانی از تمام OpenAI-compatible API ها (شامل OpenRouter و Local Models)
- **قابلیت مشاهده پیشرفته**: ردیابی عملیات با OpenTelemetry و نظارت با Langfuse

## نصب

### پیش‌نیازها

- Python 3.13
- Git
- دسترسی به OpenAI-compatible LLM Provider

1. Repository پروژه را Clone کنید:
```bash
git clone https://github.com/divar-ir/ai-doc-gen.git
cd ai-doc-gen
```

2. نصب با uv (روش پیشنهادی):
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
uv sync
```

3. یا نصب با pip:
```bash
pip install -e .
```

## شروع سریع

1. تنظیم اولیه محیط:
```bash
# کپی و ویرایش متغیرهای محیط
cp .env.sample .env

# کپی و ویرایش فایل پیکربندی
mkdir -p .ai
cp config_example.yaml .ai/config.yaml
```

2. اجرای تحلیل و تولید مستندات:
```bash
# تحلیل پروژه
uv run src/main.py analyze --repo-path .

# تولید مستندات
uv run src/main.py document --repo-path .
```

فایل‌های تولید شده در پوشه `.ai/docs/` ذخیره می‌شوند.

## استفاده

### استفاده پیشرفته

```bash
# تحلیل با حذف بخش‌های خاص
uv run src/main.py analyze --repo-path . --exclude-code-structure --exclude-data-flow

# تولید مستندات بدون برخی بخش‌ها
uv run src/main.py document --repo-path . --exclude-architecture --exclude-c4-model

# استفاده از README موجود به عنوان Context
uv run src/main.py document --repo-path . --use-existing-readme

# استفاده از فایل پیکربندی سفارشی
uv run src/main.py analyze --repo-path . --config /path/to/config.yaml

# اجرای Cronjob برای GitLab
uv run src/main.py cronjob analyze
```

## پیکربندی

ابزار به صورت خودکار فایل‌های پیکربندی را در مسیر `.ai/config.yaml` یا `.ai/config.yml` در پروژه شما جستجو می‌کند.

### گزینه‌های پیکربندی

- **تحلیل انتخابی**: امکان حذف تحلیل ساختار کد، جریان داده، وابستگی‌ها، جریان درخواست یا API
- **سفارشی‌سازی README**: تعیین اینکه کدام بخش‌ها در مستندات نهایی نمایش داده شوند
- **تنظیمات زمان‌بندی**: تعریف مسیرهای کاری و فیلترهای تازگی کامیت

برای تغییرات سریع می‌توانید از CLI Flags استفاده کنید. برای مشاهده تمام Options [`config_example.yaml`](config_example.yaml) و برای Environment Variables [`.env.sample`](.env.sample) را بررسی کنید.

## معماری سیستم

این سیستم بر پایه **Multi-Agent Architecture** ساخته شده است که از AI Agents متخصص برای انجام تحلیل‌های مختلف بهره می‌برد:

- **لایه رابط کاربری (CLI)**: نقطه ورودی سیستم و پردازش دستورات
- **لایه کنترلر (Handler)**: منطق اصلی دستورات (analyze، document، cronjob)  
- **لایه عامل (Agent)**: تحلیل هوشمند و تولید مستندات با AI
- **لایه ابزار (Tool)**: عملیات فایل سیستم و ابزارهای جانبی

### پشته فناوری

- **Python 3.13** همراه با pydantic-ai برای AI Agent Orchestration
- **OpenAI-compatible APIs** برای LLM Access (OpenAI، OpenRouter و...)
- **GitPython & python-gitlab** برای Repository Operations
- **OpenTelemetry & Langfuse** برای Observability
- **YAML + Pydantic** برای Configuration Management

## مجوز

این پروژه تحت مجوز MIT منتشر شده است - فایل [LICENSE](LICENSE) را برای جزئیات مشاهده کنید.

## تشکر و قدردانی

- Built با [pydantic-ai](https://ai.pydantic.dev/) برای AI Agent Orchestration
- پشتیبانی از Multiple LLM Providers از طریق OpenAI-compatible APIs (شامل OpenRouter)
- استفاده از [Langfuse](https://langfuse.com/) برای LLM Observability