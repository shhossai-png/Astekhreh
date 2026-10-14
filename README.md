# Astekhreh
<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>استخاره با قرآن کریم - نسخه پیشرفته</title>
    <style>
        body {
            font-family: 'Vazirmatn', Tahoma, sans-serif;
            background-color: #f4f7f6;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
        }
        .container {
            background: white;
            border-radius: 16px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.08);
            padding: 30px;
            max-width: 700px;
            width: 100%;
            text-align: center;
        }
        h1 { color: #1a5e63; margin-bottom: 10px; font-size: 1.8rem; }
        .subtitle { color: #6c757d; margin-bottom: 30px; font-size: 1rem; }
        .result-box {
            background: #f8f9fa;
            border: 1px solid #e9ecef;
            border-radius: 12px;
            padding: 25px;
            margin: 20px 0;
            min-height: 250px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            text-align: right;
            transition: all 0.3s ease;
        }
        .result-box.loading { opacity: 0.6; }
        .result-text { font-size: 1.1rem; line-height: 1.8; color: #212529; margin-bottom: 15px; }
        .result-meta {
            font-size: 0.9rem;
            color: #6c757d;
            border-top: 1px solid #e9ecef;
            padding-top: 15px;
            margin-top: 10px;
        }
        .badge {
            display: inline-block;
            padding: 6px 14px;
            border-radius: 50px;
            font-size: 0.9rem;
            font-weight: bold;
            margin-bottom: 15px;
        }
        .badge.good { background: #d4edda; color: #155724; }
        .badge.bad { background: #f8d7da; color: #721c24; }
        .badge.neutral { background: #fff3cd; color: #856404; }
        .btn {
            background: #1a5e63;
            color: white;
            border: none;
            padding: 14px 40px;
            font-size: 1.2rem;
            border-radius: 50px;
            cursor: pointer;
            transition: background 0.3s, transform 0.1s;
            font-family: inherit;
            width: 100%;
            max-width: 300px;
        }
        .btn:hover { background: #144a4e; }
        .btn:active { transform: scale(0.98); }
        .btn:disabled { background: #adb5bd; cursor: not-allowed; }
        .page-number { font-size: 0.85rem; color: #adb5bd; margin-top: 15px; }
        .verse-text {
            background: #e8f4f8;
            border-right: 4px solid #1a5e63;
            padding: 12px 18px;
            border-radius: 8px;
            margin: 15px 0;
            font-size: 1.05rem;
            color: #1a5e63;
            font-weight: 500;
        }
    </style>
    <link href="https://cdn.jsdelivr.net/gh/rastikerdar/vazirmatn@v33.003/Vazirmatn-font-face.css" rel="stylesheet" type="text/css" />
</head>
<body>

<div class="container">
    <h1>استخاره با قرآن کریم</h1>
    <p class="subtitle">نیت کنید، صلوات بفرستید و دکمه زیر را فشار دهید</p>

    <div class="result-box" id="resultBox">
        <p style="color: #6c757d; text-align: center;">برای دریافت نتیجه، روی دکمه استخاره کلیک کنید.</p>
    </div>

    <button class="btn" id="estekharehBtn">استخاره</button>
    <p class="page-number" id="pageDisplay"></p>
</div>

<script>
    // ============================================================
    // دیتابیس گسترده استخاره
    // شامل: داده‌های سایت شهید آوینی + جدول صفحه‌ای 604 صفحه
    // ============================================================

    // بخش اول: داده‌های کامل از سایت شهید آوینی (نمونه - قابل گسترش)
    const avinyDatabase = [
        {
            page: 67,
            surah: "آل عمران",
            surahNumber: 3,
            ayah: 133,
            goodBad: "حتما انجام بده",
            resultGeneral: "بسیار خوب است در انجام آن تعجیل کنید و در انجام آن حفظ موازین شرعی را رعایت کنید.",
            resultMarriage: "با این ازدواج گشایش کار پیدا میکنی دو شرط بالا فراموش نشود.",
            resultTrade: "پر منفعت و پر سود است بدو شرط یکی آنکه موازین شرعی را رعایت کنی و دوم صدقه‌ی در خوری بدهی.",
            status: "good"
        },
        {
            page: 69,
            surah: "آل عمران",
            surahNumber: 3,
            ayah: 149,
            goodBad: "انجام نده",
            resultGeneral: "بد است دچار ناراحتی زیادی خواهید شد و در راهی قرار میگیرید که بازگشتی در آن نیست.",
            resultMarriage: "هیچ خوبی و خیری در آن نیست ترک کنید.",
            resultTrade: "به حرام میافتید و فریب خواهید خورد ت رک کنید.",
            status: "bad"
        },
        {
            page: 71,
            surah: "آل عمران",
            surahNumber: 3,
            ayah: 158,
            goodBad: "اختیار با توست",
            resultGeneral: "میانه است خوب فکر کن با افراد خبره مشورت کن انجام ندهی بهتر است.",
            resultMarriage: "صبر کنید زیاد عجله نکنید مورد بهتری هم نصیبتان میشود مشورت کن انجام ندهی بهتر است.",
            resultTrade: "با افراد خبره مشورت کن آنچنان هم تعریفی ندارد.",
            status: "neutral"
        },
        {
            page: 73,
            surah: "آل عمران",
            surahNumber: 3,
            ayah: 174,
            goodBad: "انجام بده",
            resultGeneral: "خوب است با حفظ موازین شرعی اقدام کنید انشاءالله به اهدافتان خواهید رسید.",
            resultMarriage: "انشاءالله که مبارک است به‌خدا توکل کن و نمازت را اول وقت بخوان انشاءالله عاقبت بخیر میشوی.",
            resultTrade: "راستی در معامله و توکل به‌خدا رمز موفقیت است انشاءالله سود خوبی خواهید برد.",
            status: "good"
        },
        {
            page: 75,
            surah: "آل عمران",
            surahNumber: 3,
            ayah: 187,
            goodBad: "انجام نده",
            resultGeneral: "بد است ارزش شما بیش از این است اقدام نکنید.",
            resultMarriage: "همسر خوبی برای شما نخواهد بود فراموش کنید.",
            resultTrade: "به شما دروغ میگویند و فریبتان میدهند انجام ندهید.",
            status: "bad"
        },
        {
            page: 77,
            surah: "آل عمران",
            surahNumber: 3,
            ayah: 200,
            goodBad: "حتما انجام بده",
            resultGeneral: "بسیار خوب است با توکل به‌خدا اقدام کنید و در هیچ حالتی خدا را فراموش نکنید.",
            resultMarriage: "همسر خوبی است از او فرزندان خوبی نیز نصیبتان میشود انشاءالله مبارک است.",
            resultTrade: "سود خوبی نصیبتان میشود در هر کاری خدا را فراموش نکنید.",
            status: "good"
        },
        {
            page: 413,
            surah: "سجده",
            surahNumber: 32,
            ayah: 1,
            goodBad: "انجام بده",
            resultGeneral: "بسیار خوب است مورد حسرت و تحیر دیگران واقع می شوید انشاء الله نتیجه بخش است.",
            resultMarriage: "بسیار خوب است، همسری قانع، عفیف و مهربان می باشد.",
            resultTrade: "بسیار خوب است، اگر چه مشکلاتی دارد لکن به انجام دادن آن می ارزد.",
            status: "good"
        },
        {
            page: 415,
            surah: "سجده",
            surahNumber: 32,
            ayah: 1,
            goodBad: "انجام بده",
            resultGeneral: "بسیار خوب است مورد حسرت و تحیر دیگران واقع می شوید.",
            resultMarriage: "بسیار خوب است، همسری قانع، عفیف و مهربان می باشد.",
            resultTrade: "بسیار خوب است، اگر چه مشکلاتی دارد لکن به انجام دادن آن می ارزد.",
            status: "good"
        },
        {
            page: 417,
            surah: "سجده",
            surahNumber: 32,
            ayah: 21,
            goodBad: "انجام نده",
            resultGeneral: "بسیار بد است دچار عذاب شدید الهی و خسران و زیان مالی شدید خواهید شد.",
            resultMarriage: "زندگی پر درد سر و پر نزاعی خواهید داشت.",
            resultTrade: "بد است، ابتدای کار خوب پیش می رود ولی به زودی دچار مشکلاتی از طرف اطرافیانتان می شوید.",
            status: "bad"
        },
        {
            page: 601,
            surah: "عصر",
            surahNumber: 103,
            ayah: 1,
            goodBad: "حتما انجام بده",
            resultGeneral: "بسیار خوب است با توکل به‌خدا اقدام کنید انشاءالله به همه‌ی اهدافتان می‌رسید.",
            resultMarriage: "مورد مناسبی است و باعث آرامش و رفع گرفتاری‌ها از شما می‌شود.",
            resultTrade: "سود خوبی نصیبتان می‌شود در هر کاری خدا را فراموش نکنید.",
            status: "good"
        }
    ];

    // بخش دوم: جدول کامل جواب استخاره برای تمام 604 صفحه
    // (این بخش به صورت خلاصه آورده شده - برای دریافت لیست کامل به منابع مراجعه کنید)
    const pageStatusMap = {
        1: "خوب", 3: "بد", 5: "بسیار خوب", 7: "میانه", 9: "میانه بد",
        11: "میانه خوب", 13: "بد", 15: "بد", 17: "میانه خوب", 19: "بد",
        21: "بد", 23: "بد", 25: "خوب", 27: "میانه", 29: "خوب",
        31: "خوب", 33: "بسیار بد", 35: "میانه خوب", 37: "میانه", 39: "بسیار خوب",
        41: "میانه بد", 43: "میانه بد", 45: "بد", 47: "بسیار بد", 49: "میانه",
        51: "بسیار بد", 53: "بد", 55: "بسیار خوب", 57: "خوب", 59: "بد",
        61: "خوب", 63: "میانه بد", 65: "خوب", 67: "خوب", 69: "بد",
        71: "میانه", 73: "خوب", 75: "بد", 77: "خوب", 79: "خوب",
        201: "بد", 203: "بسیار خوب", 205: "بسیار خوب", 207: "خوب", 209: "بسیار بد",
        211: "بد", 213: "بد", 215: "بسیار بد", 217: "میانه بد", 219: "خوب",
        221: "میانه خوب", 223: "بد", 225: "بد", 227: "بد", 229: "بد",
        231: "بسیار بسیار بد", 233: "بسیار بد", 235: "بد", 237: "بد", 239: "خوب",
        241: "میانه بد", 243: "خوب", 245: "خوب", 247: "بسیار خوب", 249: "بسیار خوب",
        251: "بد", 253: "بسیار خوب", 255: "میانه بد", 257: "میانه خوب", 259: "خوب",
        261: "بسیار بد", 263: "بسیار خوب", 265: "خوب", 267: "میانه", 269: "بد",
        401: "بد", 403: "بسیار بد", 405: "خوب", 407: "خوب", 409: "بد",
        411: "بسیار بسیار خوب", 413: "میانه بد", 415: "بسیار خوب", 417: "بسیار بد",
        419: "میانه", 421: "بسیار خوب", 423: "بد", 425: "میانه خوب", 427: "میانه بد",
        429: "بسیار بد", 431: "میانه خوب", 433: "میانه بد", 435: "بد", 437: "میانه بد",
        439: "بد", 441: "میانه خوب", 443: "خوب", 445: "میانه بد", 447: "بد",
        449: "بسیار خوب", 451: "بد", 453: "خوب", 455: "بد", 457: "بد",
        459: "میانه خوب", 461: "بد", 463: "میانه", 465: "بد", 467: "خوب"
    };

    // تابع کمکی: تبدیل status متنی به کد رنگ
    function mapStatusToBadge(statusText) {
        if (!statusText) return { class: 'neutral', text: 'نامشخص' };
        const t = statusText.trim();
        if (t.includes('بسیار بد')) return { class: 'bad', text: t };
        if (t.includes('بد')) return { class: 'bad', text: t };
        if (t.includes('بسیار خوب') || t.includes('خوب')) return { class: 'good', text: t };
        if (t.includes('میانه')) return { class: 'neutral', text: t };
        return { class: 'neutral', text: t };
    }

    // تابع اصلی: دریافت نتیجه تصادفی از دیتابیس ترکیبی
    function getRandomResult() {
        // تصمیم‌گیری: 40% از Aviny، 60% از جدول صفحه‌ای
        const useAviny = Math.random() < 0.4 && avinyDatabase.length > 0;

        if (useAviny) {
            const idx = Math.floor(Math.random() * avinyDatabase.length);
            return avinyDatabase[idx];
        } else {
            // انتخاب تصادفی از جدول صفحه‌ای
            const pages = Object.keys(pageStatusMap).map(Number);
            const randomPage = pages[Math.floor(Math.random() * pages.length)];
            const statusText = pageStatusMap[randomPage];
            const badge = mapStatusToBadge(statusText);

            return {
                page: randomPage,
                surah: "---",
                surahNumber: 0,
                ayah: 0,
                goodBad: statusText,
                resultGeneral: `نتیجه استخاره صفحه ${randomPage}: ${statusText}`,
                resultMarriage: "برای مشاهده تفسیر کامل ازدواج، از منابع معتبر استفاده کنید.",
                resultTrade: "برای مشاهده تفسیر کامل معامله، از منابع معتبر استفاده کنید.",
                status: badge.class
            };
        }
    }

    // تابع نمایش نتیجه
    function displayResult(data) {
        let badgeClass = '';
        let badgeText = '';

        if (data.status) {
            const badge = mapStatusToBadge(data.status);
            badgeClass = badge.class;
            badgeText = badge.text || data.goodBad;
        } else {
            badgeClass = 'neutral';
            badgeText = 'نامشخص';
        }

        let surahInfo = '';
        if (data.surah && data.surah !== "---") {
            surahInfo = `
                <strong>سوره:</strong> ${data.surah} (سوره شماره ${data.surahNumber}) | 
                <strong>آیه:</strong> ${data.ayah}
            `;
        } else {
            surahInfo = '<em>این نتیجه از جدول صفحه‌ای استخراج شده است.</em>';
        }

        const html = `
            <span class="badge ${badgeClass}">${badgeText}</span>
            <div class="result-text">${data.resultGeneral || 'نتیجه‌ای یافت نشد.'}</div>
            <div class="verse-text">
                <strong>نتیجه ازدواج:</strong> ${data.resultMarriage || '---'}<br>
                <strong>نتیجه معامله:</strong> ${data.resultTrade || '---'}
            </div>
            <div class="result-meta">
                ${surahInfo}
                <br>
                <strong>صفحه:</strong> ${data.page}
            </div>
        `;

        resultBox.innerHTML = html;
        pageDisplay.textContent = `صفحه ${data.page}`;
    }

    const resultBox = document.getElementById('resultBox');
    const estekharehBtn = document.getElementById('estekharehBtn');
    const pageDisplay = document.getElementById('pageDisplay');

    estekharehBtn.addEventListener('click', () => {
        estekharehBtn.disabled = true;
        resultBox.classList.add('loading');
        resultBox.innerHTML = '<p style="text-align:center; color: #6c757d;">در حال باز کردن صفحه...</p>';

        setTimeout(() => {
            const result = getRandomResult();
            displayResult(result);
            resultBox.classList.remove('loading');
            estekharehBtn.disabled = false;
        }, 600);
    });
</script>

</body>
</html>
