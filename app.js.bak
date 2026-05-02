// ============================================================
// NEXORA — Shared App Script
// ============================================================

// Theme toggle
(function() {
  const r = document.documentElement;
  let d = window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light';
  r.setAttribute('data-theme', d);
  document.querySelectorAll('[data-theme-toggle]').forEach(function(t) {
    t.addEventListener('click', function() {
      d = d === 'dark' ? 'light' : 'dark';
      r.setAttribute('data-theme', d);
    });
  });
})();

// Sticky header
(function() {
  const header = document.getElementById('header');
  if (!header) return;
  window.addEventListener('scroll', function() {
    if (window.scrollY > 80) header.classList.add('header--scrolled');
    else header.classList.remove('header--scrolled');
  }, { passive: true });
})();

// Burger menu
(function() {
  const burger = document.getElementById('burger');
  const nav = document.getElementById('mobileNav');
  if (!burger || !nav) return;
  burger.addEventListener('click', function() {
    const open = burger.getAttribute('aria-expanded') === 'true';
    burger.setAttribute('aria-expanded', String(!open));
    nav.setAttribute('aria-hidden', String(open));
    burger.classList.toggle('is-open', !open);
    nav.classList.toggle('is-open', !open);
  });
  nav.querySelectorAll('a').forEach(function(a) {
    a.addEventListener('click', function() {
      burger.setAttribute('aria-expanded', 'false');
      nav.setAttribute('aria-hidden', 'true');
      burger.classList.remove('is-open');
      nav.classList.remove('is-open');
    });
  });
})();

// Scroll reveal
(function() {
  const els = document.querySelectorAll('[data-reveal], [data-reveal-stagger]');
  if (!els.length) return;
  const obs = new IntersectionObserver(function(entries) {
    entries.forEach(function(e) {
      if (e.isIntersecting) {
        e.target.classList.add('is-revealed');
        obs.unobserve(e.target);
      }
    });
  }, { threshold: 0.1 });
  els.forEach(function(el) { obs.observe(el); });
})();

// FAQ accordion
document.querySelectorAll('.faq-question').forEach(function(btn) {
  btn.addEventListener('click', function() {
    const item = btn.closest('.faq-item');
    const isOpen = item.classList.contains('is-open');
    document.querySelectorAll('.faq-item').forEach(function(i) {
      i.classList.remove('is-open');
      const q = i.querySelector('.faq-question');
      if (q) q.setAttribute('aria-expanded', 'false');
    });
    if (!isOpen) {
      item.classList.add('is-open');
      btn.setAttribute('aria-expanded', 'true');
    }
  });
});

// Contact form
// ── Postmark handler via /api/contact ──────────────────────────
var NX_FORMS_API = 'https://nexora-consulting.at';

function nxSubmitForm(form, btn, successText, endpoint) {
  successText = successText || 'Надіслано ✓';
  endpoint = endpoint || '/api/contact';
  btn.disabled = true;
  btn.textContent = 'Надсилання…';
  btn.style.opacity = '0.75';

  var data = new FormData(form);
  var obj = {};
  data.forEach(function(v, k) { obj[k] = v; });

  // Inject current language so server can send localized confirmation
  try { obj['_lang'] = localStorage.getItem('nx-lang') || document.documentElement.lang || 'uk'; } catch(e) { obj['_lang'] = 'uk'; }

  fetch(NX_FORMS_API + endpoint, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(obj)
  })
  .then(function(res) { return res.json(); })
  .then(function(json) {
    if (json.success) {
      btn.textContent = successText;
      btn.style.opacity = '1';
      btn.style.background = '#00A7A7';
      btn.style.color = '#fff';
      form.reset();
    } else {
      btn.textContent = 'Помилка — спробуйте ще раз';
      btn.disabled = false;
      btn.style.opacity = '1';
    }
  })
  .catch(function() {
    btn.textContent = 'Помилка — спробуйте ще раз';
    btn.disabled = false;
    btn.style.opacity = '1';
    btn.style.background = '';
  });
}

// Main contact form (#contactForm on index + kontakt pages)
var nxContactForm = document.getElementById('contactForm');
if (nxContactForm) {
  nxContactForm.addEventListener('submit', function(e) {
    e.preventDefault();
    var btn = nxContactForm.querySelector('[type=submit]');
    if (!btn) return;
    nxSubmitForm(nxContactForm, btn, 'Надіслано ✓');
  });
}

// Corporate Services inline form (#csForm)
var nxCsForm = document.getElementById('csForm');
if (nxCsForm) {
  nxCsForm.addEventListener('submit', function(e) {
    e.preventDefault();
    var btn = nxCsForm.querySelector('[type=submit]');
    if (!btn) return;
    nxSubmitForm(nxCsForm, btn, 'Запит надіслано ✓', '/api/corporate');
  });
}

// Lead magnet forms
document.querySelectorAll('.lead-magnet-form').forEach(function(form) {
  form.addEventListener('submit', function(e) {
    e.preventDefault();
    const btn = form.querySelector('[type=submit]');
    if (btn) {
      btn.textContent = 'Надіслано ✓';
      btn.disabled = true;
      btn.style.background = 'rgba(255,255,255,0.3)';
    }
  });
});

// Nav dropdown (desktop)
document.querySelectorAll('.nav-dropdown').forEach(function(dropdown) {
  const menu = dropdown.querySelector('.dropdown-menu');
  if (!menu) return;
  dropdown.addEventListener('mouseenter', function() { menu.classList.add('is-open'); });
  dropdown.addEventListener('mouseleave', function() { menu.classList.remove('is-open'); });
  // Keyboard
  dropdown.addEventListener('focusin', function() { menu.classList.add('is-open'); });
  dropdown.addEventListener('focusout', function(e) {
    if (!dropdown.contains(e.relatedTarget)) menu.classList.remove('is-open');
  });
});

// Active nav link
(function() {
  const links = document.querySelectorAll('.nav-link, .mobile-nav-link');
  const path = window.location.pathname;
  links.forEach(function(link) {
    const href = link.getAttribute('href');
    if (!href || href === '#') return;
    // Normalize href: remove relative parts and index.html
    const normalized = href.replace(/^\.\.?\/?/, '').replace(/^\.\//, '').replace(/index\.html$/, '');
    if (normalized && path.includes(normalized)) {
      link.classList.add('is-active');
    }
  });
})();

// ============================================================
// ============================================================
// i18n — Language Switcher (UA / EN / DE)
// ============================================================
(function() {
  var TRANSLATIONS = {
    uk: {
      // Nav
      'nav.home': 'Головна',
      'nav.services': 'Послуги',
      'nav.about': 'Про нас',
      'nav.contact': 'Контакти',
      'nav.insights': 'Статті',
      'nav.products': 'Продукти',
      'nav.corporate': 'Корп. послуги',
      // Nav dropdown items
      'nav.launch': 'LAUNCH — Запуск бізнесу',
      'nav.structure': 'STRUCTURE — Корпоративне структурування',
      'nav.finance': 'FINANCE & OPERATIONS',
      'nav.people': 'PEOPLE & RESIDENCY',
      'nav.scale': 'SCALE & DIGITAL',
      'nav.comply': 'COMPLY — Compliance & AML',
      // Hero
      'hero.badge': 'KONSULTING З ВІДНЯ — УКРАЇНСЬКОЮ',
      'hero.h1.line1': 'Ваш надійний',
      'hero.h1.line2': 'бізнес–партнер',
      'hero.h1.line3': 'в Австрії',
      'hero.sub': 'NEXORA допомагає українським підприємцям, IT-компаніям та міжнародним інвесторам успішно працювати в Австрії та за кордоном. Від реєстрації GmbH до корпоративного структурування у 20+ юрисдикціях.',
      'cta.free': 'Безкоштовна консультація',
      'cta.services': 'Наші послуги',
      // Stats
      'stat1.label': 'Напрямків послуг',
      'stat2.label': 'Ринок присутності',
      'stat3.label': 'Регуляторні фреймворки',
      'stat4.label': 'Адреса',
      // Sections
      'section.services.eyebrow': 'Наші послуги',
      'section.services.title': 'Підтримка на кожному етапі бізнесу',
      'section.services.sub': 'Від запуску до scale-up — NEXORA покриває весь спектр бізнес-консультування. Де потрібні ліцензовані партнери — координуємо.',
      'section.why.eyebrow': 'Наші переваги',
      'section.why.title': 'Чому обирають NEXORA',
      'section.audience.eyebrow': 'Цільові групи',
      'section.audience.title': 'Кому ми допомагаємо',
      'section.audience.sub': 'NEXORA спеціалізується на кількох ключових секторах та цільових групах клієнтів.',
      'section.process.eyebrow': 'Як ми працюємо',
      'section.process.title': 'Простий процес — надійний результат',
      'section.cta.title': 'Розкажіть нам про ваш бізнес',
      'section.cta.sub': 'Перша консультація — безкоштовна. Зв\'яжіться з нами зручним способом, і ми відповімо протягом 24 годин.',
      // Contact form
      'form.salutation': 'Звернення',
      'form.firstname': 'Ім\'я',
      'form.lastname': 'Прізвище',
      'form.title_pre': 'Титул перед ім\'ям',
      'form.title_post': 'Титул після ім\'я',
      'form.email': 'Email',
      'form.phone': 'Телефон',
      'form.service': 'Яка послуга вас цікавить?',
      'form.message': 'Ваше запитання або опис ситуації',
      'form.submit': 'Надіслати запит →',
      'form.privacy': 'Натискаючи кнопку, ви погоджуєтесь з нашою політикою конфіденційності.',
      'form.success': 'Надіслано ✓',
      // Footer
      'footer.services': 'Послуги',
      'footer.company': 'Компанія',
      'footer.jurisdictions': 'Юрисдикції',
      'footer.shelf': 'Готові компанії',
      'footer.industries': 'Індустрії',
      'footer.consultation': 'Консультація',
      'footer.legal': 'Rechtliches',
      'footer.rights': '© 2024–2026 NEXORA Unternehmensberatung GmbH. Всі права захищені.',
      // Page titles / breadcrumbs
      'page.services.title': 'Наші послуги',
      'page.about.title': 'Про нас',
      'page.contact.title': 'Контакти',
      'page.products.title': 'Продукти',
      'page.insights.title': 'Інсайти',
      // Page meta titles and descriptions
      'page.home.meta.title': 'NEXORA Unternehmensberatung — Бізнес-консультування в Австрії',
      'page.home.meta.desc': 'NEXORA — провідна консалтингова компанія у Відні для українських підприємців. Реєстрація GmbH, корпоративне структурування у 20+ юрисдикціях, податки, AML/Compliance.',
      'page.about.meta.title': 'Про нас — Команда NEXORA Unternehmensberatung',
      'page.about.meta.desc': 'Команда NEXORA: Serhii Zaitsev MBA, Oleksandr Korniiets (Master of Law, IHK Mediator) та Lukas Hertig (Швейцарія). Офіційно зареєстрована в Австрії, FN 663874k.',
      'page.contact.meta.title': 'Контакти — NEXORA Unternehmensberatung, Відень',
      'page.contact.meta.desc': "Зв'яжіться з NEXORA у Відні: Franz-Josefs-Kai 27/DG/9, 1010 Wien. Тел: +43 690 200 210 14.",
      'page.corporate.meta.title': 'Corporate Services — Реєстрація, ліцензії, готові компанії | NEXORA',
      'page.corporate.meta.desc': 'Реєстрація компаній в ЄС та офшорах, продаж готових компаній з ліцензіями EMI/VASP/Forex. 20+ юрисдикцій. Консультації з Відня.',
      'page.insights.meta.title': 'Інсайти & Аналітика — DORA, MiCA, AML, FinTech ЄС | NEXORA',
      'page.insights.meta.desc': 'Актуальна аналітика по DORA, MiCA, AML/KYC, PSD3 та регуляторних змінах для бізнесу в ЄС.',
      'page.products.meta.title': 'Цифрові продукти — CRM, DORA, Compliance Tools | NEXORA',
      'page.products.meta.desc': 'Цифрові продукти NEXORA для бізнесу: CRM для консалтингу, DORA-compliance platform, Jurisdictions Screener.',
      'page.services.meta.title': 'Послуги — NEXORA Unternehmensberatung',
      'page.services.meta.desc': 'Шість напрямків бізнес-консультування в Австрії: LAUNCH, STRUCTURE, FINANCE & OPS, PEOPLE, SCALE, COMPLY.',
      'page.contact.eyebrow': 'Зв\'яжіться з нами',
      'page.contact.h1': 'Ми поруч — напишіть або зателефонуйте',
      'page.contact.sub': 'Перша консультація безкоштовна. Відповімо протягом 24 годин у робочі дні.',
      'page.about.eyebrow': 'Наша команда',
      'page.about.h1': 'Команда, яка розуміє два світи',
      'page.about.sub': 'NEXORA заснована українцями у Відні. Ми знаємо, як думає австрійський бізнес — і як думають наші клієнти з України. Цей міст між двома культурами — наша головна перевага.',
      'page.about.story.eyebrow': 'Наша історія',
      'page.about.story.h2': 'Народжені потребою — зростаємо разом з клієнтами',
      'page.about.cta.eyebrow': 'Починаємо?',
      'page.services.h1': 'Шість напрямків. Один консультант.',
      'page.services.sub': 'NEXORA надає весь спектр дозволених послуг Unternehmensberatung — від запуску бізнесу до compliance та цифрової трансформації.',
      'page.services.cta.eyebrow': 'Готові розпочати?',
      'page.products.h1': 'Цифровий інструментарій для серйозного бізнесу',
      'page.cta.eyebrow': 'Готові розпочати?',
      'page.gruendung.meta.title': 'LAUNCH — Відкриття та запуск бізнесу в Австрії | NEXORA',
      'page.gruendung.meta.desc': 'GmbH, FlexKapG, Einzelunternehmen в Австрії — від вибору форми до Firmenbuch та Gewerbeschein. Пакети від €710.',
      'page.structure.meta.title': 'STRUCTURE — КІК, PE-ризики, холдинги AT–UA | NEXORA',
      'page.structure.meta.desc': 'КІК/CFC-аналіз, PE-ризики, холдингові структури, DTA AT–UA. Пакети SCAN €690 · SHIELD €1,490 · FULL €2,800.',
      'page.finance.meta.title': 'FINANCE & OPERATIONS — Банківський рахунок, cashflow, KPI | NEXORA',
      'page.finance.meta.desc': 'Відкриття Geschäftskonto, KYC-пакет, cashflow-planning, KPI-системи, управлінська звітність.',
      'page.people.meta.title': 'PEOPLE & RESIDENCY — RWR-Karte, HR консультування в Австрії | NEXORA',
      'page.people.meta.desc': 'RWR-Karte підприємця, Niederlassungsbewilligung, MA 35, HR консультування. ORIENTATION від €490.',
      'page.comply.meta.title': 'COMPLY — AML/KYC, MiCA/CASP, DORA, Sanctions | NEXORA',
      'page.comply.meta.desc': 'AML/CFT-програма, MiCA/CASP ліцензування, DORA implementation, sanctions screening для бізнесу в ЄС.',
      'page.scale.meta.title': 'SCALE & DIGITAL — Вихід на DACH, M&A, AI-автоматизація | NEXORA',
      'page.scale.meta.desc': 'Market Entry DACH, M&A Advisory, інвестиційний раунд, CRM/ERP advisory, AI автоматизація бізнес-процесів.',
      // Leistungen subpage hero headings
      'page.gruendung.h1': 'LAUNCH — Відкриття та запуск бізнесу в Австрії',
      'page.gruendung.sub': 'Від вибору форми (GmbH / FlexKapG / Einzelunternehmen) до отримання всіх реєстраційних документів. Прозоро, швидко, без зайвих кроків.',
      'page.structure.h1': 'STRUCTURE — Корпоративне структурування та cross-border advisory',
      'page.structure.sub': 'Холдингові структури AT–UA, питання постійного представництва та контрольованих іноземних компаній (КІК/CFC). Угоди DTA Австрія–Україна.',
      'page.finance.h1': 'FINANCE & OPERATIONS — Фінанси та операційна підтримка',
      'page.finance.sub': 'Відкриття Geschäftskonto, cashflow-planning, KPI-системи та управлінська звітність для вашого австрійського бізнесу.',
      'page.people.h1': 'PEOPLE & RESIDENCY — Персонал та вид на проживання',
      'page.people.sub': 'RWR-Karte підприємця, Niederlassungsbewilligung, MA 35 та HR-консультування для українців в Австрії.',
      'page.comply.h1': 'COMPLY — Compliance, AML/KYC та регуляторний advisory',
      'page.comply.sub': 'AML: штраф FMA до €1M за відсутність програми. MiCA: операція без CASP-ліцензії незаконна. DORA: ICT-ризики — обов\'язкова звітність.',
      'page.scale.h1': 'SCALE & DIGITAL — Вихід на ринок DACH, зростання та цифрова трансформація',
      'page.scale.sub': 'UA-компанія готова до DACH-ринку або стратегічних угод? NEXORA координує M&A advisory, market entry та digital growth.',
      // Aria-labels
      'aria.main.nav': 'Головна навігація',
      'aria.lang.switch': 'Перемкнути мову',
      'aria.lang.uk': 'Українська',
      'aria.lang.en': 'English',
      'aria.lang.de': 'Deutsch',
      'aria.theme.switch': 'Перемкнути тему',
      'aria.menu.open': 'Відкрити меню',
      // About page (6a)
      'about.hero.h1': 'Команда',
      'about.hero.sub': 'Люди, яким ви довіряєте свій бізнес',
      'about.values.title': 'Наші цінності',
      'about.values.sub': 'На чому ми будуємо кожен проект',
      'about.val1.title': 'Надійність',
      'about.val1.text': 'Дотримуємось дедлайнів і обіцянок. Якщо ми сказали «за 3 тижні» — значить за 3 тижні. Ваш бізнес не може чекати.',
      'about.val2.title': 'Результат',
      'about.val2.text': 'Ми орієнтовані на результат, а не на процес. Наша мета — щоб ваш бізнес успішно функціонував і розвивався.',
      'about.accred.title': 'Акредитація та ліцензування',
      'about.accred.sub': 'Офіційна акредитація та реєстрація',
      'about.firmenbuch.text': 'Товариство зареєстроване у Firmenbuch Österreich під номером FN 663874 k. Повна правова прозорість та юридична особа австрійського права.',
      'about.uid.text': 'Зареєстровані платники ПДВ в Австрії з UID ATU82669239. Всі виставлені рахунки містять обовʼязкові реквізити відповідно до австрійського законодавства.',
      'about.partners.title': 'Партнери',
      'about.partners.sub': 'Наша партнерська мережа',
      'about.partners.text': 'Для надання комплексних послуг NEXORA співпрацює з перевіреними партнерами: австрійськими нотаріусами, аудиторами та юридичними фірмами.',
      'about.partner1.title': 'Австрійські нотаріуси',
      'about.partner1.desc': 'Реєстрація компаній та нотаріальні послуги',
      'about.cta.title': 'Поговоримо про ваш бізнес',
      'about.cta.sub': "Перша консультація — безкоштовно. Ми зв'яжемось з вами протягом 24 годин.",
      'about.cta.btn': "Зв'язатись з нами",
      'about.cta.btn2': 'Наші послуги',
      // Contact page (6b)
      'contact.process.title': 'Як це працює',
      'contact.process.sub': 'Процес консультації',
      'contact.step1.num': '1',
      'contact.step1.title': 'Надсилаєте запит',
      'contact.step1.text': 'Заповніть форму або зателефонуйте нам. Відповідаємо протягом 24 годин у робочі дні.',
      'contact.step2.num': '2',
      'contact.step2.title': 'Безкоштовна консультація',
      'contact.step2.text': '30-хвилинна первинна консультація — онлайн або в нашому офісі у Відні. Без зобовʼязань.',
      'contact.step3.num': '3',
      'contact.step3.title': 'Індивідуальна пропозиція',
      'contact.step3.text': 'Після консультації ви отримаєте конкретну пропозицію з термінами та вартістю.',
      'contact.faq.title': 'FAQ',
      'contact.faq.sub': 'Швидкі відповіді',
      'contact.faq.q1': 'Чи є перша консультація справді безкоштовною?',
      'contact.faq.a1': 'Так. Перша 30-хвилинна консультація завжди безкоштовна.',
      'contact.faq.q2': 'Консультації проводяться лише у Відні чи онлайн також?',
      'contact.faq.a2': 'Ми консультуємо як в офісі у Відні (Franz-Josefs-Kai 27), так і онлайн через Zoom або MS Teams.',
      'contact.faq.q3': 'Якою мовою проводяться консультації?',
      'contact.faq.a3': 'Ми консультуємо українською, Deutsch та англійською мовами.',
      'contact.office.title': 'Бізнес-консультування та корпоративні послуги з Відня',
      'contact.office.address': 'Franz-Josefs-Kai 27/DG/9, 1010 Wien, Österreich',
      // Corporate services (6c)
      'cs.disclaimer': 'Дані захищені відповідно до GDPR. Ми не передаємо їх третім особам.',
      'cs.hero.h1': 'Міжнародне корпоративне структурування з Відня',
      'cs.hero.sub': 'Реєстрація компаній, готові фірми з ліцензіями, EMI/VASP/CASP-ліцензування та банківські рахунки — повний цикл під ключ. Реальні ціни, перевірені регуляторні дані.',
      'cs.hero.shelf.btn': 'Готові компанії →',
      'cs.section.title': 'Що входить у Corporate Services',
      'cs.section.desc': 'Шість напрямків — від реєстрації нової компанії до продажу готової фірми з ліцензією та відкритим рахунком.',
      'cs.shelf.title': 'Готові компанії (Shelf)',
      'cs.shelf.desc': 'Власний портфель — без брокерів. Ми самостійно реєструємо та підтримуємо компанії у ключових юрисдикціях.',
      'cs.data.updated': 'Дані по юрисдикціях оновлено квітень 2026. Консультаційні послуги — не юридичні.',
      // Footer extras
      'footer.tagline': 'Бізнес-консультування та корпоративні послуги з Відня',
      'footer.datenschutz': 'Захист даних',
      'footer.cookies': 'Політика Cookies',
      'footer.haftung': 'Haftungsausschluss',
      // Insights page
      'ins.eyebrow': 'Інсайти & Аналітика',
      'ins.h1.line1': 'Регуляторика, яка',
      'ins.h1.em': 'справді важлива',
      'ins.sub': 'Практична аналітика щодо DORA, MiCA, AML/KYC, PSD3 та ключових регуляторних змін для бізнесу в ЄС.',
      'ins.stat1': 'Фахових аналізів',
      'ins.stat3': 'Мови публікацій', // updated
      'ins.filter.all': 'Всі теми',
      'ins.filter.markt': 'Вихід на ринок',
      'ins.featured.badge': 'Нова аналітика',
      'ins.art0.cat': 'Огляд ринку',
      'ins.art0.title': '2026: Регуляторний тиск як нова реальність',
      'ins.art0.excerpt': 'Платежі, криптоактиви та цифрові інвестиції у 2026 році стикаються з щільним регуляторним середовищем. DORA, MiCA, PSD3 та CSRD одночасно створюють нові зобов\'язання для компаній в ЄС.',
      'ins.tag.regulatory': 'Регуляторика',
      'ins.tag.enforcement': 'Enforcement',
      'ins.read': 'хв. читання',
      'ins.read.full': 'Читати повністю →',
      'ins.grid.title': 'Всі аналізи',
      'ins.grid.sub': 'Актуальні матеріали щодо регуляторних змін в ЄС',
      'ins.weiterlesen': 'Читати далі →',
      'ins.cat.market': 'Вихід на ринок',
      'ins.cat.regulatory': 'Регуляторика',
      'ins.cat.enforcement': 'Enforcement',
      'ins.cat.strategy': 'Compliance',
      'ins.cat.strat': 'Стратегія',
      'ins.art1.title': 'DORA: Від класичної ІТ-безпеки до доведеної резилієнтності критичних сервісів',
      'ins.art1.excerpt': 'Digital Operational Resilience Act встановлює єдині стандарти управління ІКТ-ризиками в ЄС — включно з ланцюжком постачання ІКТ та управлінням сторонніми провайдерами.',
      'ins.art2.title': 'Вихід на ринок ЄС 2026: MiCA та PSD3/PSR як нові регуляторні рамки',
      'ins.art2.excerpt': 'ЄС у 2026 році має майже завершену нормативну базу для криптоактивів і платежів. Що це означає для стратегій виходу FinTech- і Crypto-компаній на ринок.',
      'ins.art3.title': 'EBA інтегрує MiCA у систему індикаторів ризику',
      'ins.art3.excerpt': '28 січня 2026 року EBA опублікувала оновлений перелік індикаторів оцінки ризику — і вперше системно інтегрувала MiCA до наглядової аналітики.',
      'ins.art4.title': 'MiCA без ілюзій: як регулятори ЄС насправді зупиняють токен-проєкти',
      'ins.art4.excerpt': 'З 30 грудня 2024 року MiCA повністю набула чинності для емітентів криптоактивів. Що насправді роблять наглядові органи і що це означає для компаній у DACH-регіоні.',
      'ins.art5.title': 'MiCA Compliance Playbook: що криптокомпанії мають зробити зараз',
      'ins.art5.excerpt': 'Структурований огляд конкретних кроків для дотримання регламенту MiCA в ЄС — з фокусом на DACH-ринок і практичними рекомендаціями.',
      'ins.art6.title': 'Стратегія зростання для регульованих компаній: DORA, MiCA і CSRD як можливість',
      'ins.art6.excerpt': 'Регуляторні вимоги DORA, MiCA та CSRD часто сприймаються як тягар. Ця стаття показує, як стратегічно використати їх як конкурентну перевагу.',
      'ins.art7.title': 'MiCA Tokenomics & Governance: що насправді мають враховувати емітенти',
      'ins.art7.excerpt': 'Регламент MiCA висуває чіткі вимоги до дизайну токеноміки та структур управління. Стаття пояснює, що конкретно мають впровадити емітенти криптоактивів.',
      'ins.art8.title': 'Санкційний скринінг & PEP-перевірки: практичний посібник для компаній в ЄС',
      'ins.art8.excerpt': 'Санкційний скринінг та PEP-перевірки є обов\'язковими для багатьох компаній. Посібник пояснює правові вимоги і показує, як побудувати ефективну систему скринінгу.',
      'ins.art9.title': 'AML/KYC Blueprint: побудова майбутньостійкого Compliance-фреймворку',
      'ins.art9.excerpt': 'Структурований Blueprint для побудови надійного AML/KYC Compliance-фреймворку — адаптований до актуальних вимог ЄС і реалій регульованих компаній.',
      'ins.stat2.label': 'Період охоплення',
      'ins.filter.aml': 'AML / KYC',
      'ins.filter.tax': 'Податки',
      'ins.filter.launch': 'Вихід на ринок',
      'ins.filter.strategy': 'Стратегія',
      'ins.a1.cat': 'Стратегія / Огляд',
      'ins.a1.title': '2026: Regulatory Overhang як нова нормальність',
      'ins.a1.excerpt': 'Платежі, криптоактиви та цифрові інвестиції у 2026 році стикаються з щільним регуляторним середовищем. MiCA, DORA, AI Act, PSD3/PSR та реформи ринку капіталу впроваджуються поетапно — огляд ключових тез для бізнесу в ЄС.',
      'ins.a2.cat': 'DORA',
      'ins.a2.title': 'DORA: від класичної ІТ-безпеки до підтвердженої стійкості критично важливих наскрізних сервісів',
      'ins.a2.excerpt': 'DORA оцінює не наявність фреймворку на папері, а здатність критичних сервісів реально продовжувати роботу під навантаженням — включно з ланцюжком постачання ІКТ та сторонніми провайдерами.',
      'ins.a3.cat': 'MiCA / FinTech',
      'ins.a3.title': 'Вихід на ринок ЄС у 2026 році: MiCA та PSD3/PSR як нова регуляторна рамка для FinTech і Crypto',
      'ins.a3.excerpt': '2026 рік стає практичним «фільтром»: без ліцензії CASP більшість криптобізнес-моделей не зможуть працювати в ЄС. Австрія — привабливий хаб з чітким наглядом FMA та паспортингом по всьому ЄС.',
      'ins.a4.cat': 'MiCA / EBA',
      'ins.a4.title': 'EBA інтегрує MiCA у свою систему індикаторів ризику: від другорядної теми до ядра наглядової аналітики',
      'ins.a4.excerpt': '28 січня 2026 EBA вперше включила крипторизики до єдиного набору KRI — на рівні з класичними банківськими ризиками. Практичні наслідки для банків, FinTech та CASP з гібридними моделями.',
      'ins.a5.cat': 'MiCA / Enforcement',
      'ins.a5.title': 'MiCA без ілюзій: як регулятори ЄС насправді зупиняють токен-проєкти — з фокусом на регіон DACH',
      'ins.a5.excerpt': 'З 30 грудня 2024 MiCA повністю набула чинності. Після закінчення перехідних положень регулятори зобов\'язані виводити неавторизованих провайдерів з ринку. Що це означає для CASP у DACH-регіоні.',
      'ins.a6.cat': 'Стратегія',
      'ins.a6.title': 'Стратегія зростання 2026–2027: Як інтегрувати DORA, MiCA та CSRD у ваш бізнес-план',
      'ins.a6.excerpt': 'DORA, MiCA та CSRD більше не є темами для юридичного відділу — вони визначають доступ до ринків та фінансування. Посібник з інтеграції регуляторних вимог у стратегію зростання на 2026–2027 роки.',
      'ins.a7.cat': 'AML / Санкції',
      'ins.a7.title': 'Санкційний та перевірочний скринінг для МСП: мінімальна конфігурація для зниження ризиків санкцій ЄС',
      'ins.a7.excerpt': 'Санкції ЄС стосуються кожного МСП, що працює з третіми країнами або чутливими товарами. Чотири центральні контрольні точки та мінімальна конфігурація без дорогого ПЗ — практичний посібник.',
      'ins.a8.cat': 'AML / KYC',
      'ins.a8.title': 'AML/KYC Blueprint 2026: Від оцінки ризиків до онбордингу та поточного моніторингу (EU AMLR & 6AMLD)',
      'ins.a8.excerpt': 'Повний наскрізний процес AML/KYC для FinTech, криптопровайдерів та платіжних систем: шаблони оцінки ризиків, дерева рішень для онбордингу, посібник з моніторингу та ролі управління.',
      'ins.a9.cat': 'MiCA / Tech',
      'ins.a9.title': 'MiCA Compliance Playbook для команд розробки продуктів та технологій',
      'ins.a9.excerpt': 'MiCA — це не лише юридична тема. Для продакт-менеджерів та CTO регламент означає конкретні вимоги до систем, архітектури даних та IT-управління. Playbook для технічних команд CASP у DACH.',
      'ins.a10.cat': 'MiCA / Токеноміка',
      'ins.a10.title': 'MiCA-сумісна токеноміка та управління для криптопроєктів ЄС (фокус: регіон DACH)',
      'ins.a10.excerpt': 'MiCA вимагає не лише прозорої документації, а й економічно доброчесного дизайну токеноміки. Управління конфліктами інтересів, структури governance та вимоги до White Paper — для CASP та емітентів.',
      'ins.a11.cat': 'MiCA / Інвестори',
      'ins.a11.title': 'Брифінг для інвесторів MiCA/CASP — Ризики та можливості',
      'ins.a11.excerpt': 'Для інвесторів, що розглядають вкладення в CASP або токен-проєкти ЄС: як MiCA змінює регуляторний ландшафт, що означає ліцензійний статус та на які «червоні прапори» звертати увагу.',
      'ins.a12.cat': 'MiCA / Австрія',
      'ins.a12.title': 'MiCA в Австрії: Дорожня карта для ліцензії CASP для криптопроєктів',
      'ins.a12.excerpt': 'Практична дорожня карта для кінця 2025 — початку 2026 року: контрольні списки документів, типові помилки, часовий горизонт ліцензування FMA та коли достатньо партнерської моделі замість власної ліцензії.',
      'ins.a13.cat': 'DORA / Австрія',
      'ins.a13.title': 'DORA в Австрії/ЄС: Хто підпадає під дію та 10 кроків до мінімальної відповідності',
      'ins.a13.excerpt': 'DORA діє з 17 січня 2025 для майже всіх фінансових установ ЄС. Ця стаття пояснює, хто підпадає під регламент, та пропонує 10 конкретних кроків для досягнення мінімальної відповідності.',
      'ins.a14.cat': 'Податки / Зарплата',
      'ins.a14.title': 'Податки та додаткові витрати на заробітну плату в Австрії 2026: Посібник для засновників на перші 90 днів',
      'ins.a14.excerpt': 'Перші 90 днів після заснування ТОВ в Австрії є вирішальними з податкової точки зору. Корпоративний податок, ПДВ, додаткові витрати на зарплату та реалістичний 90-денний план дій.',
      'ins.a15.cat': 'Реєстрація / KYC',
      'ins.a15.title': 'Банківський рахунок для іноземних засновників в Австрії 2026: KYC-чекліст, підтвердження UBO та бізнес-кейс',
      'ins.a15.excerpt': 'Відкриття рахунку для GmbH в Австрії є найкритичнішим кроком — більш трудомістким, ніж саме внесення до Firmenbuch. KYC-чекліст, UBO-підтвердження та підготовка бізнес-кейсу для банку.',
      'ins.a16.cat': 'Реєстрація / Австрія',
      'ins.a16.title': 'Заснування компанії та взаємодія з органами влади в Австрії 2026: Firmenbuch, реєстрація Gewerbe, UID',
      'ins.a16.excerpt': 'Firmenbuch, орган з питань промислової діяльності та податкова служба працюють послідовно — кожна помилка затримує реєстрацію на тижні. Покрокова процедура, документи та типові запити органів.',
      'ins.a17.cat': 'Реєстрація / Структура',
      'ins.a17.title': 'GmbH проти EPU проти OG: яка правова форма справді підходить міжнародним засновникам?',
      'ins.a17.excerpt': 'Вибір правової форми в Австрії впливає на особисту відповідальність, можливості фінансування та привабливість для інвесторів. Дерево рішень та п\'ять реальних бізнес-кейсів для міжнародних засновників.',
      'ins.a18.cat': 'Вихід на ринок',
      'ins.a18.title': 'Вихід на ринок Австрії 2025–2026: покрокова інструкція для міжнародних засновників',
      'ins.a18.excerpt': 'Відень — ворота на ринок DACH з 100+ млн споживачів. Покрокова інструкція від вибору правової форми до відкриття рахунку — з чек-листами, локальними партнерами та цифровими процесами.',
      'ins.lm.h2': 'Регуляторний брифінг',
      'ins.lm.h2em': 'прямо у вашу поштову скриньку',
      'ins.lm.desc': 'Щомісячний дайджест найважливіших регуляторних змін — стисло, практично і без зайвого жаргону. Для керівників, які мають бути в курсі подій.',
      'ins.lm.perk1': 'DORA, MiCA, AML/KYC, PSD3 — щомісяця підібрано',
      'ins.lm.perk2': 'Практичні поради та рекомендації до дій',
      'ins.lm.perk3': 'Безкоштовно, скасування будь-коли',
      'ins.lm.formtitle': 'Підписатися зараз',
      'ins.lm.formhint': 'Для компаній та фахівців в ЄС',
      'ins.lm.name': 'Ваше ім\'я',
      'ins.lm.email': 'Корпоративна e-mail адреса',
      'ins.lm.size': 'Розмір компанії',
      'ins.lm.s1': '1–10 співробітників',
      'ins.lm.s2': '11–50 співробітників',
      'ins.lm.s3': '51–200 співробітників',
      'ins.lm.s4': '200+ співробітників',
      'ins.lm.btn': 'Підписатися на брифінг →',
      'dok.h1': 'Шаблони договорів і правові документи',
      'dok.sub': 'Професійні шаблони для ваших ділових відносин — складені відповідно до австрійського та європейського права. Усі документи адаптуються індивідуально і не є юридичною консультацією.',
      'dok.grid.title': 'Доступні шаблони',
      'dok.grid.sub': 'Усі шаблони відповідають стану квітня 2026 р. та орієнтовані на австрійське право та GDPR/регламенти ЄС.',
      'dok.doc1.title': 'Договір про надання консультаційних послуг',
      'dok.doc1.desc': 'Стандартний договір між NEXORA та клієнтом. Регулює обсяг послуг, гонорар, конфіденційність, відповідальність та розірвання за австрійським правом (ABGB, UGB).',
      'dok.doc2.title': 'Угода про нерозголошення (NDA)',
      'dok.doc2.desc': 'NDA для захисту конфіденційної інформації в рамках консультаційних проектів, due diligence та M&A-транзакцій. Можливе односторонє або двостороннє виконання.',
      'dok.doc3.title': 'Договір на обробку даних (AVV)',
      'dok.doc3.desc': 'Обов\'язковий договір відповідно до ст. 28 GDPR для обробки персональних даних за дорученням. Містить усі обов\'язкові положення GDPR та технічно-організаційні заходи (TOM).',
      'dok.doc4.title': 'Політика конфіденційності',
      'dok.doc4.desc': 'Повна Політика конфіденційності відповідно до GDPR для операторів веб-сайтів та компаній. Регулює правові підстави обробки, права суб\'єктів даних, Cookie-менеджмент та міжнародну передачу даних.',
      'dok.doc5.title': 'Загальні умови надання послуг (AGB)',
      'dok.doc5.desc': 'Повні AGB для консалтингових компаній за австрійським правом. Охоплює предмет послуг, гонорари, відповідальність, конфіденційність, захист даних, розірвання та особливі положення для споживачів.',
      'dok.doc6.title': 'Проектна угода та комерційна пропозиція',
      'dok.doc6.desc': 'Шаблон для проектних угод із описом послуг, графіком, міlestonами та планом платежів. Незабаром.',
      'dok.disclaimer.title': 'Важливе застереження',
      'dok.disclaimer.text': 'Ці шаблони є виключно відправною точкою і не замінюють індивідуальну юридичну консультацію. NEXORA рекомендує перевіряти всі документи перед підписанням та адаптувати їх до вашої конкретної ситуації.',
      'dok.cta.title': 'Індивідуальне складання договорів',
      'dok.cta.desc': 'Потребуєте договорів, адаптованих до вашої ситуації? Наша юридична команда складає індивідуальні документи для M&A-транзакцій, міжнародних структурувань та регуляторних вимог.',
      'ins.lm.disclaimer': 'Без спаму. Без передачі третім особам. Захист даних відповідно до GDPR.',
      'hero.eyebrow': 'Business Consulting у Відні',
      'hero.h1': 'Ваш надійний|бізнес-партнер|в Австрії',
      'hero.sub': 'NEXORA допомагає українським підприємцям, IT-компаніям та міжнародним інвесторам успішно працювати в Австрії та за кордоном. Від реєстрації GmbH до корпоративного структурування у 20+ юрисдикціях.',
      'hero.stat1': 'Напрямків послуг',
      'hero.stat2': 'Ринок присутності',
      'hero.stat3.sub': 'MiCA · AML · PSD3',
      'cta.services': 'Наші послуги',
      'section.services.eyebrow': 'Наші послуги',
      'section.services.h2': 'Підтримка на кожному етапі бізнесу',
      'svc.launch.h3': 'Відкриття та запуск бізнесу',
      'svc.launch.p': 'GmbH, FlexKapG, Einzelunternehmen — вибір форми, документи, Firmenbuch, Gewerbeanmeldung. Координація з нотаріусом та Steuerberater.',
      'svc.structure.h3': 'Корпоративне структурування',
      'svc.structure.p': 'КІК/CFC-аналіз, PE-ризики, холдингові структури AT–UA, аналіз DTA-договорів. Стратегія для multi-jurisdiction бізнесу.',
      'svc.finance.h3': 'Фінансова прозорість',
      'svc.finance.p': 'Cashflow-planning, KPI-системи, управлінська звітність, Unternehmensbewertung, Sanierungsberatung — управлінський аналіз бізнесу.',
      'svc.people.h3': 'Команда та міграційний статус',
      'svc.people.p': 'HR-консультування, RWR-Karte, Niederlassungsbewilligung, форми зайнятості, ÖGK. Для підприємців та їх команд в Австрії.',
      'svc.scale.h3': 'Digital Growth та Scale-up',
      'svc.scale.p': 'Стратегічний advisory, виведення на ринки ЄС/DACH, M&A-підтримка, партнерський пошук, digital-трансформація операційних процесів.',
      'svc.comply.h3': 'Compliance та AML',
      'svc.comply.p': 'DORA, MiCA, AML/KYC, PSD3 — регуляторний супровід для FinTech та Crypto компаній. Побудова compliance-фреймворків.',
      'cta.more': 'Детальніше →',
      // WHY NEXORA
      'why1.title': 'Ліцензована компанія в Австрії',
      'why1.text': 'NEXORA Unternehmensberatung GmbH зареєстрована у Firmenbuch (FN 663874k), має Gewerbeberechtigung GISA №38919244 на „Unternehmensberatung einschließlich der Unternehmensorganisation" та підтверджений UID ATU82669239.',
      'why2.title': 'Знаємо обидві системи зсередини',
      'why2.text': 'Команда має практичний досвід роботи з австрійським та українським законодавством. Ми розуміємо специфіку пострадянського бізнесу, українського податкового права та австрійського корпоративного права одночасно.',
      'why3.title': 'Міжнародна мережа партнерів',
      'why3.text': 'Через перевірених партнерів маємо доступ до 20+ юрисдикцій: ЄС, Дубай, Сінгапур, Швейцарія та інші. Координуємо, коли потрібна спеціалізована ліцензована допомога.',
      'why4.title': 'Повний цикл — від реєстрації до масштабування',
      'why4.text': 'LAUNCH → ACCOUNTS → STRUCTURE → RESIDENCY → SCALE → COMPLY. Ви не переходите від консультанта до консультанта — весь шлях з одним партнером.',
      'why5.title': 'Відповідність австрійському законодавству',
      'why5.text': 'Діємо строго в межах Berufsbild Unternehmensberatung (§136 GewO). Там, де потрібен Steuerberater або Rechtsanwalt — відкрито про це говоримо та координуємо перевірених партнерів.',
      'why6.title': 'Конфіденційність та NDA-підхід',
      'why6.text': 'Перед будь-яким обміном чутливою інформацією підписуємо NDA. Особливо критично для структурних питань, КІК та defence/regulated бізнесу.',
      // AUDIENCE
      'aud1.title': 'Українці в Австрії',
      'aud1.text': 'Підприємці та фахівці, які переїхали до Австрії та хочуть відкрити або перенести свій бізнес. Говоримо рідною мовою.',
      'aud2.title': 'IT / Tech компанії',
      'aud2.text': 'Технологічні компанії, що шукають надійну юрисдикцію в ЄС для масштабування. Розуміємо специфіку IT-бізнесу.',
      'aud3.title': 'FinTech / Crypto / FOREX',
      'aud3.text': 'Компанії у фінансовій сфері, що потребують EMI, VASP або FOREX ліцензій. Підтримка з MiCA-дедлайном 2026.',
      'aud4.title': 'E-commerce',
      'aud4.text': 'Інтернет-магазини та маркетплейси, що бажають отримати австрійський чи ЄС юридичну адресу для роботи на європейських ринках.',
      'aud5.title': 'Міжнародні підприємці',
      'aud5.text': 'Бізнесмени з інших країн, що обирають Австрію як платформу для входження на ринок ЄС. Допомагаємо з нуля.',
      'aud6.title': 'Defence / MilTech',
      'aud6.text': 'Компанії в оборонному та технологічному секторі, що потребують корпоративного структурування та міжнародного комплаєнсу.',
      // PROCESS
      'proc1.title': 'Консультація',
      'proc1.text': 'Безкоштовна первинна консультація. Обговорюємо ваші цілі та ситуацію. Знайомимось.',
      'proc2.title': 'Аналіз ситуації',
      'proc2.text': 'Аналізуємо потреби, готуємо оптимальну стратегію та план дій. Пропонуємо найкраще рішення.',
      'proc3.title': 'Реалізація',
      'proc3.text': 'Впроваджуємо план. Супроводжуємо на кожному кроці — від документів до реєстрації.',
      'proc4.title': 'Підтримка',
      'proc4.text': 'Після завершення проекту залишаємось поруч за потреби. Ongoing підтримка в координації з ліцензованими партнерами.',
      // MiCA BANNER
      'mica.eyebrow': '⚠️ Терміновий дедлайн',
      'mica.title': 'Дедлайн MiCA: 1 липня 2026 — залишилось менше 3 місяців',
      'mica.sub': 'Якщо ваша компанія працює з криптоактивами в ЄС, вам необхідно отримати VASP-ліцензію або MiCA-авторизацію до 1 липня 2026. Ми допоможемо вам встигнути.',
      'mica.cta': 'Дізнатись більше про Corporate Services →',
      // REVIEWS
      'reviews.eyebrow': 'Відгуки',
      'reviews.title': 'Що кажуть наші клієнти',
      'rev1.text': '"NEXORA допомогла нам відкрити GmbH у Відні всього за три тижні. Спілкування лише українською — жодного бар\'єру. Сергій пояснив кожен крок, ми знали що відбувається на кожному етапі."',
      'rev1.name': 'Олег Мартиненко',
      'rev1.role': 'IT-підприємець, Відень',
      'rev2.text': '"Нашій e-commerce компанії потрібна була юридична адреса в ЄС. NEXORA структурувала все оптимально — з мінімальним податковим навантаженням та повним комплаєнсом. Дуже рекомендую Oleksandr за юридичну частину."',
      'rev2.name': 'Наталія Коваленко',
      'rev2.role': 'E-commerce, засновниця',
      'rev3.text': '"Ми займаємось FinTech і нам була потрібна VASP-ліцензія в ЄС. Команда NEXORA знала всі нюанси регуляторного процесу. Завдяки їхній роботі ми отримали ліцензію вчасно і без зайвого стресу."',
      'rev3.name': 'Василь Бондаренко',
      'rev3.role': 'FinTech CEO, Відень',
      // CONTACT CTA
      'section.cta.eyebrow': 'Починаємо разом',
      'contact.phone.label': 'Телефон',
      'contact.email.label': 'Email',
      // COOKIE BANNER
      'cookie.title': 'Ми використовуємо cookies',
      'cookie.body': 'Ми використовуємо файли cookie для покращення роботи сайту та аналізу трафіку. Переглянути <a href="cookies/index.html">Cookie-richtlinii</a>.',
      'cookie.reject': 'Лише необхідні',
      'cookie.accept': 'Прийняти все',
    },
    en: {
      'nav.home': 'Home',
      'nav.services': 'Services',
      'nav.about': 'About us',
      'nav.contact': 'Contact',
      'nav.insights': 'Insights',
      'nav.products': 'Products',
      'nav.corporate': 'Corporate Services',
      'nav.launch': 'LAUNCH — Business Setup',
      'nav.structure': 'STRUCTURE — Corporate Structuring',
      'nav.finance': 'FINANCE & OPERATIONS',
      'nav.people': 'PEOPLE & RESIDENCY',
      'nav.scale': 'SCALE & DIGITAL',
      'nav.comply': 'COMPLY — Compliance & AML',
      'hero.badge': 'CONSULTING FROM VIENNA — IN ENGLISH',
      'hero.h1.line1': 'Your trusted',
      'hero.h1.line2': 'business partner',
      'hero.h1.line3': 'in Austria',
      'hero.sub': 'NEXORA helps Ukrainian entrepreneurs, IT companies and international investors operate successfully in Austria and abroad. From GmbH registration to corporate structuring in 20+ jurisdictions.',
      'cta.free': 'Free consultation',
      'cta.services': 'Our services',
      'stat1.label': 'Service areas',
      'stat2.label': 'Market presence',
      'stat3.label': 'Regulatory frameworks',
      'stat4.label': 'Address',
      'section.services.eyebrow': 'Our Services',
      'section.services.title': 'Support at every stage of your business',
      'section.services.sub': 'From launch to scale-up — NEXORA covers the full spectrum of business consulting. Licensed partners are coordinated where required.',
      'section.why.eyebrow': 'Our advantages',
      'section.why.title': 'Why choose NEXORA',
      'section.audience.eyebrow': 'Target groups',
      'section.audience.title': 'Who we help',
      'section.audience.sub': 'NEXORA specializes in several key sectors and client groups.',
      'section.process.eyebrow': 'How we work',
      'section.process.title': 'Simple process — reliable results',
      'section.cta.title': 'Ready to get started?',
      'section.cta.sub': 'Contact us for a free consultation',
      'form.salutation': 'Salutation',
      'form.firstname': 'First name',
      'form.lastname': 'Last name',
      'form.title_pre': 'Title (prefix)',
      'form.title_post': 'Title (suffix)',
      'form.email': 'Email',
      'form.phone': 'Phone',
      'form.service': 'Which service interests you?',
      'form.message': 'Your question or description',
      'form.submit': 'Send request →',
      'form.privacy': 'By clicking the button, you agree to our privacy policy.',
      'form.success': 'Sent ✓',
      'footer.services': 'Services',
      'footer.company': 'Company',
      'footer.jurisdictions': 'Jurisdictions',
      'footer.shelf': 'Shelf companies',
      'footer.industries': 'Industries',
      'footer.consultation': 'Consultation',
      'footer.legal': 'Legal',
      'footer.rights': '© 2024–2026 NEXORA Unternehmensberatung GmbH. All rights reserved.',
      'page.services.title': 'Our Services',
      'page.about.title': 'About us',
      'page.contact.title': 'Contact',
      'page.products.title': 'Products',
      'page.insights.title': 'Insights',
      // Page meta titles and descriptions
      'page.home.meta.title': 'NEXORA Unternehmensberatung — Business Consulting in Austria',
      'page.home.meta.desc': 'NEXORA — leading consulting firm in Vienna for Ukrainian entrepreneurs. GmbH registration, corporate structuring in 20+ jurisdictions, tax, AML/Compliance.',
      'page.about.meta.title': 'About us — NEXORA Unternehmensberatung Team',
      'page.about.meta.desc': 'The NEXORA team: Serhii Zaitsev MBA, Oleksandr Korniiets (Master of Law, IHK Mediator) and Lukas Hertig (Switzerland). Officially registered in Austria, FN 663874k.',
      'page.contact.meta.title': 'Contact — NEXORA Unternehmensberatung, Vienna',
      'page.contact.meta.desc': 'Contact NEXORA in Vienna: Franz-Josefs-Kai 27/DG/9, 1010 Wien. Phone: +43 690 200 210 14.',
      'page.corporate.meta.title': 'Corporate Services — Company Registration, Licences, Shelf Companies | NEXORA',
      'page.corporate.meta.desc': 'Company registration in the EU and offshore, shelf companies with EMI/VASP/Forex licences. 20+ jurisdictions. Consulting from Vienna.',
      'page.insights.meta.title': 'Insights & Analytics — DORA, MiCA, AML, EU FinTech | NEXORA',
      'page.insights.meta.desc': 'Current analysis on DORA, MiCA, AML/KYC, PSD3 and regulatory changes for businesses in the EU.',
      'page.products.meta.title': 'Digital Products — CRM, DORA, Compliance Tools | NEXORA',
      'page.products.meta.desc': 'NEXORA digital products for business: CRM for consulting, DORA compliance platform, Jurisdictions Screener.',
      'page.services.meta.title': 'Services — NEXORA Unternehmensberatung',
      'page.services.meta.desc': 'Six business consulting areas in Austria: LAUNCH, STRUCTURE, FINANCE & OPS, PEOPLE, SCALE, COMPLY.',
      'page.contact.eyebrow': 'Contact us',
      'page.contact.h1': 'We are here — write or call us',
      'page.contact.sub': 'First consultation is free. We respond within 24 hours on business days.',
      'page.about.eyebrow': 'Our team',
      'page.about.h1': 'A team that understands two worlds',
      'page.about.sub': 'NEXORA was founded by Ukrainians in Vienna. We understand how Austrian business thinks — and how our clients from Ukraine think. This bridge between two cultures is our main advantage.',
      'page.about.story.eyebrow': 'Our story',
      'page.about.story.h2': 'Born from necessity — growing together with clients',
      'page.about.cta.eyebrow': 'Ready to start?',
      'page.services.h1': 'Six areas. One consultant.',
      'page.services.sub': 'NEXORA provides the full range of permitted Unternehmensberatung services — from business launch to compliance and digital transformation.',
      'page.services.cta.eyebrow': 'Ready to start?',
      'page.products.h1': 'Digital toolkit for serious business',
      'page.cta.eyebrow': 'Ready to start?',
      'page.gruendung.meta.title': 'LAUNCH — Business Setup in Austria | NEXORA',
      'page.gruendung.meta.desc': 'GmbH, FlexKapG, Einzelunternehmen in Austria — from choosing a legal form to Firmenbuch and Gewerbeschein. Packages from €710.',
      'page.structure.meta.title': 'STRUCTURE — CFC, PE risks, AT–UA holdings | NEXORA',
      'page.structure.meta.desc': 'CFC/PE analysis, holding structures, DTA AT–UA. Packages SCAN €690 · SHIELD €1,490 · FULL €2,800.',
      'page.finance.meta.title': 'FINANCE & OPERATIONS — Business account, cashflow, KPI | NEXORA',
      'page.finance.meta.desc': 'Geschäftskonto opening, KYC package, cashflow planning, KPI systems, management reporting.',
      'page.people.meta.title': 'PEOPLE & RESIDENCY — RWR-Karte, HR consulting in Austria | NEXORA',
      'page.people.meta.desc': 'Entrepreneurial RWR-Karte, Niederlassungsbewilligung, MA 35, HR consulting. ORIENTATION from €490.',
      'page.comply.meta.title': 'COMPLY — AML/KYC, MiCA/CASP, DORA, Sanctions | NEXORA',
      'page.comply.meta.desc': 'AML/CFT programme, MiCA/CASP licensing, DORA implementation, sanctions screening for businesses in the EU.',
      'page.scale.meta.title': 'SCALE & DIGITAL — DACH expansion, M&A, AI automation | NEXORA',
      'page.scale.meta.desc': 'Market Entry DACH, M&A Advisory, investment round, CRM/ERP advisory, AI automation of business processes.',
      // Leistungen subpage hero headings
      'page.gruendung.h1': 'LAUNCH — Business Setup in Austria',
      'page.gruendung.sub': 'From choosing a legal form (GmbH / FlexKapG / Einzelunternehmen) to receiving all registration documents. Transparent, fast, no unnecessary steps.',
      'page.structure.h1': 'STRUCTURE — Corporate Structuring & Cross-Border Advisory',
      'page.structure.sub': 'Holding structures AT–UA, permanent establishment questions and CFC/controlled foreign company issues. DTA Austria–Ukraine treaties.',
      'page.finance.h1': 'FINANCE & OPERATIONS — Finance & Operational Support',
      'page.finance.sub': 'Geschäftskonto opening, cashflow planning, KPI systems and management reporting for your Austrian business.',
      'page.people.h1': 'PEOPLE & RESIDENCY — Staff & Residence Permits',
      'page.people.sub': 'Entrepreneurial RWR-Karte, Niederlassungsbewilligung, MA 35 and HR consulting for Ukrainians in Austria.',
      'page.comply.h1': 'COMPLY — Compliance, AML/KYC & Regulatory Advisory',
      'page.comply.sub': 'AML: FMA fine up to €1M for missing programme. MiCA: operating without CASP licence is illegal. DORA: ICT risks — mandatory reporting.',
      'page.scale.h1': 'SCALE & DIGITAL — DACH Market Entry, Growth & Digital Transformation',
      'page.scale.sub': 'Austrian company ready for the DACH market or strategic deals? NEXORA coordinates M&A advisory, market entry and digital growth.',
      // Aria-labels
      'aria.main.nav': 'Main navigation',
      'aria.lang.switch': 'Switch language',
      'aria.lang.uk': 'Ukrainian',
      'aria.lang.en': 'English',
      'aria.lang.de': 'German',
      'aria.theme.switch': 'Switch theme',
      'aria.menu.open': 'Open menu',
      // About page (6a)
      'about.hero.h1': 'Our Team',
      'about.hero.sub': 'The people you trust with your business',
      'about.values.title': 'Our Values',
      'about.values.sub': 'The foundation of every project we take on',
      'about.val1.title': 'Reliability',
      'about.val1.text': 'We meet deadlines and keep our promises. If we said "in 3 weeks" — it means in 3 weeks. Your business cannot wait.',
      'about.val2.title': 'Results',
      'about.val2.text': 'We are results-oriented, not process-oriented. Our goal is for your business to operate and grow successfully.',
      'about.accred.title': 'Accreditation & Licensing',
      'about.accred.sub': 'Official accreditation and registration',
      'about.firmenbuch.text': 'The company is registered in the Firmenbuch Österreich under FN 663874 k. Full legal transparency and legal entity under Austrian law.',
      'about.uid.text': 'Registered VAT payers in Austria with UID ATU82669239. All invoices contain mandatory details in accordance with Austrian law.',
      'about.partners.title': 'Partners',
      'about.partners.sub': 'Our partner network',
      'about.partners.text': 'To provide comprehensive services, NEXORA works with trusted partners: Austrian notaries, auditors and law firms.',
      'about.partner1.title': 'Austrian Notaries',
      'about.partner1.desc': 'Company registration and notarial services',
      'about.cta.title': "Let's talk about your business",
      'about.cta.sub': 'First consultation is free. We will contact you within 24 hours.',
      'about.cta.btn': 'Contact us',
      'about.cta.btn2': 'Our services',
      // Contact page (6b)
      'contact.process.title': 'How it works',
      'contact.process.sub': 'The consultation process',
      'contact.step1.num': '1',
      'contact.step1.title': 'Submit your request',
      'contact.step1.text': 'Fill in the form or call us. We respond within 24 hours on working days.',
      'contact.step2.num': '2',
      'contact.step2.title': 'Free consultation',
      'contact.step2.text': '30-minute initial consultation — online or at our Vienna office. No obligation.',
      'contact.step3.num': '3',
      'contact.step3.title': 'Individual proposal',
      'contact.step3.text': 'After the consultation you will receive a specific proposal with timeline and pricing.',
      'contact.faq.title': 'FAQ',
      'contact.faq.sub': 'Quick answers',
      'contact.faq.q1': 'Is the first consultation really free?',
      'contact.faq.a1': 'Yes. The first 30-minute consultation is always free of charge.',
      'contact.faq.q2': 'Are consultations only in Vienna or also online?',
      'contact.faq.a2': 'We consult both at our Vienna office (Franz-Josefs-Kai 27) and online via Zoom or MS Teams.',
      'contact.faq.q3': 'In which languages are consultations conducted?',
      'contact.faq.a3': 'We consult in Ukrainian, German and English.',
      'contact.office.title': 'Business consulting and corporate services from Vienna',
      'contact.office.address': 'Franz-Josefs-Kai 27/DG/9, 1010 Wien, Austria',
      // Corporate services (6c)
      'cs.disclaimer': 'Data protected in accordance with GDPR. We do not share it with third parties.',
      'cs.hero.h1': 'International Corporate Structuring from Vienna',
      'cs.hero.sub': 'Company registration, shelf companies with licenses, EMI/VASP/CASP licensing and bank accounts — full turnkey cycle. Real prices, verified regulatory data.',
      'cs.hero.shelf.btn': 'Shelf Companies →',
      'cs.section.title': 'What is included in Corporate Services',
      'cs.section.desc': 'Six service areas — from registering a new company to selling a shelf company with license and open bank account.',
      'cs.shelf.title': 'Shelf Companies',
      'cs.shelf.desc': 'Our own portfolio — no brokers. We independently register and maintain companies in key jurisdictions.',
      'cs.data.updated': 'Jurisdiction data updated April 2026. Advisory services — not legal advice.',
      // Footer extras
      'footer.tagline': 'Business consulting and corporate services from Vienna',
      'footer.datenschutz': 'Privacy Policy',
      'footer.cookies': 'Cookie Policy',
      'footer.haftung': 'Disclaimer',
      // Insights page
      'ins.eyebrow': 'Insights & Analytics',
      'ins.h1.line1': 'Regulation that',
      'ins.h1.em': 'truly matters',
      'ins.sub': 'Practical insights on DORA, MiCA, AML/KYC, PSD3 and key regulatory changes for businesses in the EU.',
      'ins.stat1': 'Expert analyses',
      'ins.stat3': 'Publication languages', // updated
      'ins.filter.all': 'All topics',
      'ins.filter.markt': 'Market entry',
      'ins.featured.badge': 'Latest analysis',
      'ins.art0.cat': 'Market overview',
      'ins.art0.title': '2026: Regulatory Overhang as the New Normal',
      'ins.art0.excerpt': 'Payments, crypto assets and digital investments face a dense regulatory environment in 2026. DORA, MiCA, PSD3 and CSRD simultaneously create new obligations for companies in the EU.',
      'ins.tag.regulatory': 'Regulatory',
      'ins.tag.enforcement': 'Enforcement',
      'ins.read': 'min. read',
      'ins.read.full': 'Read full article →',
      'ins.grid.title': 'All analyses',
      'ins.grid.sub': 'Current articles on regulatory developments in the EU',
      'ins.weiterlesen': 'Read more →',
      'ins.cat.market': 'Market entry',
      'ins.cat.regulatory': 'Regulatory',
      'ins.cat.enforcement': 'Enforcement',
      'ins.cat.strategy': 'Compliance',
      'ins.cat.strat': 'Strategy',
      'ins.art1.title': 'DORA: From Classic IT Security to Proven Resilience of Critical Services',
      'ins.art1.excerpt': 'The Digital Operational Resilience Act establishes uniform standards for ICT risk management in the EU — including ICT supply chain and third-party provider management.',
      'ins.art2.title': 'EU Market Entry 2026: MiCA and PSD3/PSR as the New Regulatory Framework',
      'ins.art2.excerpt': 'The EU enters 2026 with a largely complete rulebook for crypto assets and payments. What this means for market entry strategies of FinTech and crypto companies.',
      'ins.art3.title': 'EBA Integrates MiCA into Its Risk Indicator System',
      'ins.art3.excerpt': 'On January 28, 2026, the EBA published an updated list of Risk Assessment Indicators — and for the first time systematically integrated MiCA into supervisory analytics.',
      'ins.art4.title': 'MiCA Without Illusions: How EU Regulators Actually Stop Token Projects',
      'ins.art4.excerpt': 'Since December 30, 2024, MiCA has been fully in force for issuers of crypto assets. What supervisory authorities actually do and what this means for DACH companies.',
      'ins.art5.title': 'MiCA Compliance Playbook: What Crypto Companies Must Do Now',
      'ins.art5.excerpt': 'A structured overview of the concrete steps crypto companies must take to comply with MiCA regulation in the EU — with a focus on DACH.',
      'ins.art6.title': 'Growth Strategy for Regulated Companies: DORA, MiCA and CSRD as Opportunity',
      'ins.art6.excerpt': 'Regulatory requirements like DORA, MiCA and CSRD are often seen as a burden. This article shows how to strategically use them as a competitive advantage.',
      'ins.art7.title': 'MiCA Tokenomics & Governance: What Issuers Really Need to Consider',
      'ins.art7.excerpt': 'MiCA sets clear requirements for tokenomics design and governance structures. This article explains what issuers of crypto assets must specifically implement.',
      'ins.art8.title': 'Sanctions Screening & PEP Checks: Practical Guide for Companies in the EU',
      'ins.art8.excerpt': 'Sanctions screening and PEP checks are mandatory for many companies. This guide explains legal requirements and shows how to build an effective screening system.',
      'ins.art9.title': 'AML/KYC Blueprint: Building a Future-Proof Compliance Framework',
      'ins.art9.excerpt': 'A structured blueprint for building a robust AML/KYC compliance framework — adapted to current EU requirements and the realities of regulated companies.',
      'ins.stat2.label': 'Coverage period',
      'ins.filter.aml': 'AML / KYC',
      'ins.filter.tax': 'Taxes',
      'ins.filter.launch': 'Market entry',
      'ins.filter.strategy': 'Strategy',
      'ins.a1.cat': 'Strategy / Overview',
      'ins.a1.title': '2026: Regulatory Overhang as the New Normal',
      'ins.a1.excerpt': 'Payments, crypto assets and digital investments face a dense regulatory environment in 2026. MiCA, DORA, AI Act, PSD3/PSR and capital markets reform are being rolled out in phases — a strategic briefing for EU businesses.',
      'ins.a2.cat': 'DORA',
      'ins.a2.title': 'DORA: From Classic IT Security to Proven Resilience of Critical End-to-End Services',
      'ins.a2.excerpt': 'DORA evaluates not whether a framework exists on paper, but whether critical services can actually continue operating under load — including the full ICT supply chain and third-party providers.',
      'ins.a3.cat': 'MiCA / FinTech',
      'ins.a3.title': 'EU Market Entry 2026: MiCA and PSD3/PSR as the New Regulatory Framework for FinTech and Crypto',
      'ins.a3.excerpt': '2026 acts as a practical filter: without a CASP licence, most crypto business models cannot operate in the EU. Austria remains an attractive hub with clear FMA oversight and EU-wide passporting.',
      'ins.a4.cat': 'MiCA / EBA',
      'ins.a4.title': 'EBA Integrates MiCA into Its Risk Indicator System: From Side Note to Core Supervisory Analytics',
      'ins.a4.excerpt': 'On January 28, 2026, the EBA for the first time included crypto risks in a unified KRI set — on par with classic banking risks. Practical implications for banks, FinTech and hybrid CASP models.',
      'ins.a5.cat': 'MiCA / Enforcement',
      'ins.a5.title': 'MiCA Without Illusions: How EU Regulators Actually Stop Token Projects — Focus on DACH',
      'ins.a5.excerpt': 'Since December 30, 2024, MiCA is fully in force. Once transitional provisions expire, regulators are obliged to remove unauthorised providers from the market. What this means for CASP in the DACH region.',
      'ins.a6.cat': 'Strategy',
      'ins.a6.title': 'Growth Strategy 2026–2027: How to Integrate DORA, MiCA and CSRD into Your Business Plan',
      'ins.a6.excerpt': 'DORA, MiCA and CSRD are no longer just legal department topics — they determine access to markets and financing. A guide to integrating regulatory requirements into growth strategy for 2026–2027.',
      'ins.a7.cat': 'AML / Sanctions',
      'ins.a7.title': 'Sanctions and Verification Screening for SMEs: Minimum Configuration to Reduce EU Sanctions Risk',
      'ins.a7.excerpt': 'EU sanctions affect every SME working with third countries or sensitive goods. Four central checkpoints and a minimum setup without expensive software — a practical guide for small businesses.',
      'ins.a8.cat': 'AML / KYC',
      'ins.a8.title': 'AML/KYC Blueprint 2026: From Risk Assessment to Onboarding and Ongoing Monitoring (EU AMLR & 6AMLD)',
      'ins.a8.excerpt': 'A complete end-to-end AML/KYC process for FinTech, crypto providers and payment systems: risk assessment templates, onboarding decision trees, monitoring guide and governance roles.',
      'ins.a9.cat': 'MiCA / Tech',
      'ins.a9.title': 'MiCA Compliance Playbook for Product and Technology Teams',
      'ins.a9.excerpt': 'MiCA is not just a legal topic. For product managers and CTOs, the regulation means concrete requirements for systems, data architecture and IT governance. A playbook for CASP tech teams in DACH.',
      'ins.a10.cat': 'MiCA / Tokenomics',
      'ins.a10.title': 'MiCA-Compliant Tokenomics and Governance for EU Crypto Projects (Focus: DACH Region)',
      'ins.a10.excerpt': 'MiCA requires not just transparent documentation but economically sound tokenomics design. Conflict of interest management, governance structures and White Paper requirements — for CASP and issuers.',
      'ins.a11.cat': 'MiCA / Investors',
      'ins.a11.title': 'MiCA/CASP Investor Briefing — Risks and Opportunities',
      'ins.a11.excerpt': 'For investors considering stakes in EU CASP or token projects: how MiCA changes the regulatory landscape, what a licence status means and what red flags to watch for.',
      'ins.a12.cat': 'MiCA / Austria',
      'ins.a12.title': 'MiCA in Austria: Roadmap to CASP Licence for Crypto Projects',
      'ins.a12.excerpt': 'A practical roadmap for late 2025 to early 2026: documentation checklists, common errors, FMA licensing timelines and when a partner model suffices instead of obtaining your own licence.',
      'ins.a13.cat': 'DORA / Austria',
      'ins.a13.title': 'DORA in Austria/EU: Who Is Subject to It and 10 Steps to Minimum Compliance',
      'ins.a13.excerpt': 'DORA has been in force since January 17, 2025 for almost all EU financial institutions. This article explains who is covered and offers 10 concrete steps to achieve minimum compliance.',
      'ins.a14.cat': 'Taxes / Payroll',
      'ins.a14.title': 'Taxes and Payroll Costs in Austria 2026: Founder\'s Guide for the First 90 Days',
      'ins.a14.excerpt': 'The first 90 days after forming a GmbH in Austria are critical from a tax perspective. Corporate tax, VAT, payroll costs and a realistic 90-day action plan for international founders.',
      'ins.a15.cat': 'Incorporation / KYC',
      'ins.a15.title': 'Bank Account for Foreign Founders in Austria 2026: KYC Checklist, UBO Confirmation and Business Case',
      'ins.a15.excerpt': 'Opening a bank account for a GmbH in Austria is the most critical step — more demanding than the Firmenbuch registration itself. KYC checklist, UBO confirmation and business case preparation.',
      'ins.a16.cat': 'Incorporation / Austria',
      'ins.a16.title': 'Company Formation and Government Liaison in Austria 2026: Firmenbuch, Gewerbe Registration, UID',
      'ins.a16.excerpt': 'Firmenbuch, trade authorities and tax office work sequentially — every mistake delays registration by weeks. Step-by-step procedure, required documents and typical queries from Austrian authorities.',
      'ins.a17.cat': 'Incorporation / Structure',
      'ins.a17.title': 'GmbH vs EPU vs OG: Which Legal Form Really Suits International Founders?',
      'ins.a17.excerpt': 'The choice of legal form in Austria affects personal liability, financing options and investor attractiveness. A decision tree and five real business cases for international founders.',
      'ins.a18.cat': 'Market Entry',
      'ins.a18.title': 'Market Entry Austria 2025–2026: Step-by-Step Guide for International Founders',
      'ins.a18.excerpt': 'Vienna is the gateway to the DACH market with 100+ million consumers. A step-by-step guide from choosing a legal form to opening a bank account — with checklists and local partners.',
      'ins.lm.h2': 'Regulatory Briefing',
      'ins.lm.h2em': 'directly to your inbox',
      'ins.lm.desc': 'Monthly summary of the most important regulatory developments — concise, practical and without jargon. For decision-makers who need to stay informed.',
      'ins.lm.perk1': 'DORA, MiCA, AML/KYC, PSD3 — monthly curated',
      'ins.lm.perk2': 'Practical tips and action recommendations',
      'ins.lm.perk3': 'Free, cancel anytime',
      'ins.lm.formtitle': 'Subscribe now',
      'ins.lm.formhint': 'For companies and professionals in the EU',
      'ins.lm.name': 'Your name',
      'ins.lm.email': 'Business email address',
      'ins.lm.size': 'Company size',
      'ins.lm.s1': '1–10 employees',
      'ins.lm.s2': '11–50 employees',
      'ins.lm.s3': '51–200 employees',
      'ins.lm.s4': '200+ employees',
      'ins.lm.btn': 'Subscribe to briefing →',
      'dok.h1': 'Contract Templates & Legal Documents',
      'dok.sub': 'Professional templates for your business relationships — drafted under Austrian and EU law. All documents are individually adapted and do not constitute legal advice.',
      'dok.grid.title': 'Available Templates',
      'dok.grid.sub': 'All templates reflect the status as of April 2026 and are aligned with Austrian law and GDPR/EU regulations.',
      'dok.doc1.title': 'Consulting Agreement',
      'dok.doc1.desc': 'Standard agreement for consulting services between NEXORA and clients. Governs scope, fees, confidentiality, liability and termination under Austrian law (ABGB, UGB).',
      'dok.doc2.title': 'Non-Disclosure Agreement (NDA)',
      'dok.doc2.desc': 'NDA for protecting confidential information in consulting projects, due diligence and M&A transactions. Configurable as bilateral or unilateral.',
      'dok.doc3.title': 'Data Processing Agreement (DPA)',
      'dok.doc3.desc': 'Legally required agreement under Art. 28 GDPR for processing personal data on behalf of a controller. Includes all mandatory GDPR provisions and technical-organisational measures (TOMs).',
      'dok.doc4.title': 'Privacy Policy',
      'dok.doc4.desc': 'Complete Privacy Policy under GDPR for website operators and companies. Covers legal bases for processing, data subject rights, cookie management and international data transfers.',
      'dok.doc5.title': 'General Terms & Conditions (GTC)',
      'dok.doc5.desc': 'Complete GTC for consulting companies under Austrian law. Covers scope of services, fees, liability, confidentiality, data protection, termination and consumer protection provisions.',
      'dok.doc6.title': 'Project Agreement & Proposal',
      'dok.doc6.desc': 'Template for project-specific agreements with service description, timeline, milestones and payment plan. Coming soon.',
      'dok.disclaimer.title': 'Important Notice',
      'dok.disclaimer.text': 'These templates serve as a starting point only and do not replace individual legal advice. NEXORA recommends having all documents reviewed by a lawyer before signing and adapting them to your specific situation.',
      'dok.cta.title': 'Individual Contract Drafting',
      'dok.cta.desc': 'Need contracts tailored to your specific situation? Our legal team drafts individual documents for M&A transactions, international structuring and regulatory requirements.',
      'ins.lm.disclaimer': 'No spam. No data sharing. Privacy protected under GDPR.',
      'hero.eyebrow': 'Business Consulting in Vienna',
      'hero.h1': 'Your Reliable|Business Partner|in Austria',
      'hero.sub': 'NEXORA helps Ukrainian entrepreneurs, IT companies and international investors succeed in Austria and beyond. From GmbH registration to corporate structuring in 20+ jurisdictions.',
      'hero.stat1': 'Service directions',
      'hero.stat2': 'Market presence',
      'hero.stat3.sub': 'MiCA · AML · PSD3',
      'cta.services': 'Our Services',
      'section.services.eyebrow': 'Our Services',
      'section.services.h2': 'Support at Every Stage of Your Business',
      'svc.launch.h3': 'Business Setup & Launch',
      'svc.launch.p': 'GmbH, FlexKapG, sole proprietorship — entity selection, documents, Firmenbuch registration, trade license. Full coordination with notary and tax advisor.',
      'svc.structure.h3': 'Corporate Structuring',
      'svc.structure.p': 'CFC/KIK analysis, PE risk assessment, AT–UA holding structures, DTA treaty analysis. Strategy for multi-jurisdiction businesses.',
      'svc.finance.h3': 'Financial Transparency',
      'svc.finance.p': 'Cashflow planning, KPI systems, management reporting, business valuation, restructuring advisory — management analysis for your business.',
      'svc.people.h3': 'Team & Residency',
      'svc.people.p': 'HR consulting, Red-White-Red Card, residence permit, employment structures, ÖGK. For entrepreneurs and their teams in Austria.',
      'svc.scale.h3': 'Digital Growth & Scale-up',
      'svc.scale.p': 'Strategic advisory, EU/DACH market entry, M&A support, partner search, digital transformation of operational processes.',
      'svc.comply.h3': 'Compliance & AML',
      'svc.comply.p': 'DORA, MiCA, AML/KYC, PSD3 — regulatory guidance for FinTech and Crypto companies. Building compliance frameworks.',
      'cta.more': 'Learn more →',
      // WHY NEXORA
      'why1.title': 'Licensed Company in Austria',
      'why1.text': 'NEXORA Unternehmensberatung GmbH is registered in the Firmenbuch (FN 663874k), holds Gewerbeberechtigung GISA №38919244 for „Unternehmensberatung einschließlich der Unternehmensorganisation" and confirmed UID ATU82669239.',
      'why2.title': 'We Know Both Systems from the Inside',
      'why2.text': 'Our team has hands-on experience with both Austrian and Ukrainian law. We understand the specifics of post-Soviet business, Ukrainian tax law and Austrian corporate law simultaneously.',
      'why3.title': 'International Partner Network',
      'why3.text': 'Through trusted partners, we have access to 20+ jurisdictions: EU, Dubai, Singapore, Switzerland and more. We coordinate whenever specialist licensed assistance is needed.',
      'why4.title': 'Full Cycle — from Registration to Scale',
      'why4.text': "LAUNCH → ACCOUNTS → STRUCTURE → RESIDENCY → SCALE → COMPLY. You don't move from consultant to consultant — the entire journey with one partner.",
      'why5.title': 'Compliance with Austrian Law',
      'why5.text': 'We operate strictly within the Berufsbild Unternehmensberatung (§136 GewO). Where a Steuerberater or Rechtsanwalt is required, we are transparent about it and coordinate trusted partners.',
      'why6.title': 'Confidentiality and NDA Approach',
      'why6.text': 'Before any exchange of sensitive information, we sign an NDA. Especially critical for structural matters, CFC issues and defence/regulated business.',
      // AUDIENCE
      'aud1.title': 'Ukrainians in Austria',
      'aud1.text': 'Entrepreneurs and professionals who have relocated to Austria and want to open or transfer their business. We speak your language.',
      'aud2.title': 'IT / Tech Companies',
      'aud2.text': 'Technology companies looking for a reliable EU jurisdiction for scaling. We understand the specifics of IT business.',
      'aud3.title': 'FinTech / Crypto / FOREX',
      'aud3.text': 'Companies in the financial sector that require EMI, VASP or FOREX licences. Support with the 2026 MiCA deadline.',
      'aud4.title': 'E-commerce',
      'aud4.text': 'Online shops and marketplaces that want to obtain an Austrian or EU legal address for operating in European markets.',
      'aud5.title': 'International Entrepreneurs',
      'aud5.text': 'Business people from other countries choosing Austria as a platform to enter the EU market. We help from scratch.',
      'aud6.title': 'Defence / MilTech',
      'aud6.text': 'Companies in the defence and technology sector that require corporate structuring and international compliance.',
      // PROCESS
      'proc1.title': 'Consultation',
      'proc1.text': 'Free initial consultation. We discuss your goals and situation. Getting acquainted.',
      'proc2.title': 'Situation Analysis',
      'proc2.text': 'We analyse your needs, prepare the optimal strategy and action plan. We propose the best solution.',
      'proc3.title': 'Implementation',
      'proc3.text': 'We implement the plan. We accompany you at every step — from documents to registration.',
      'proc4.title': 'Support',
      'proc4.text': 'After the project is completed, we remain available as needed. Ongoing support in coordination with licensed partners.',
      // MiCA BANNER
      'mica.eyebrow': '⚠️ Urgent Deadline',
      'mica.title': 'MiCA Deadline: 1 July 2026 — Less Than 3 Months Left',
      'mica.sub': 'If your company operates with crypto assets in the EU, you need to obtain a VASP licence or MiCA authorisation by 1 July 2026. We will help you make it in time.',
      'mica.cta': 'Learn more about Corporate Services →',
      // REVIEWS
      'reviews.eyebrow': 'Testimonials',
      'reviews.title': 'What Our Clients Say',
      'rev1.text': '"NEXORA helped us open a GmbH in Vienna in just three weeks. Communication in Ukrainian only — no barrier whatsoever. Serhii explained every step, we knew what was happening at each stage."',
      'rev1.name': 'Oleg Martynenko',
      'rev1.role': 'IT Entrepreneur, Vienna',
      'rev2.text': '"Our e-commerce company needed a legal address in the EU. NEXORA structured everything optimally — with minimum tax burden and full compliance. Highly recommend Oleksandr for the legal part."',
      'rev2.name': 'Natalia Kovalenko',
      'rev2.role': 'E-commerce, Founder',
      'rev3.text': '"We are in FinTech and needed a VASP licence in the EU. The NEXORA team knew all the nuances of the regulatory process. Thanks to their work, we received the licence on time and without unnecessary stress."',
      'rev3.name': 'Vasyl Bondarenko',
      'rev3.role': 'FinTech CEO, Vienna',
      // CONTACT CTA
      'section.cta.eyebrow': "Let's Get Started",
      'contact.phone.label': 'Phone',
      'contact.email.label': 'Email',
      // COOKIE BANNER
      'cookie.title': 'We use cookies',
      'cookie.body': 'We use cookies to improve website performance and analyse traffic. View our <a href="cookies/index.html">Cookie Policy</a>.',
      'cookie.reject': 'Necessary only',
      'cookie.accept': 'Accept all',
    },
    de: {
      'nav.home': 'Startseite',
      'nav.services': 'Leistungen',
      'nav.about': 'Über uns',
      'nav.contact': 'Kontakt',
      'nav.insights': 'Insights',
      'nav.products': 'Produkte',
      'nav.corporate': 'Corporate Services',
      'nav.launch': 'LAUNCH — Unternehmensgründung',
      'nav.structure': 'STRUCTURE — Konzernstrukturierung',
      'nav.finance': 'FINANCE & OPERATIONS',
      'nav.people': 'PEOPLE & RESIDENCY',
      'nav.scale': 'SCALE & DIGITAL',
      'nav.comply': 'COMPLY — Compliance & AML',
      'hero.badge': 'BERATUNG AUS WIEN — AUF DEUTSCH',
      'hero.h1.line1': 'Ihr zuverlässiger',
      'hero.h1.line2': 'Geschäftspartner',
      'hero.h1.line3': 'in Österreich',
      'hero.sub': 'NEXORA unterstützt ukrainische Unternehmer, IT-Unternehmen und internationale Investoren bei ihrer erfolgreichen Tätigkeit in Österreich und im Ausland. Von der GmbH-Gründung bis zur Konzernstrukturierung in 20+ Jurisdiktionen.',
      'cta.free': 'Kostenlose Beratung',
      'cta.services': 'Unsere Leistungen',
      'stat1.label': 'Leistungsbereiche',
      'stat2.label': 'Marktpräsenz',
      'stat3.label': 'Regulatorische Rahmen',
      'stat4.label': 'Adresse',
      'section.services.eyebrow': 'Unsere Leistungen',
      'section.services.title': 'Unterstützung in jeder Unternehmensphase',
      'section.services.sub': 'Von der Gründung bis zum Scale-up — NEXORA deckt das gesamte Spektrum der Unternehmensberatung ab. Bei Bedarf koordinieren wir lizenzierte Partner.',
      'section.why.eyebrow': 'Unsere Vorteile',
      'section.why.title': 'Warum NEXORA wählen',
      'section.audience.eyebrow': 'Zielgruppen',
      'section.audience.title': 'Wem wir helfen',
      'section.audience.sub': 'NEXORA ist auf mehrere Schlüsselsektoren und Kundengruppen spezialisiert.',
      'section.process.eyebrow': 'So arbeiten wir',
      'section.process.title': 'Einfacher Prozess — zuverlässige Ergebnisse',
      'section.cta.title': 'Bereit anzufangen?',
      'section.cta.sub': 'Kontaktieren Sie uns für eine kostenlose Erstberatung',
      'form.salutation': 'Anrede',
      'form.firstname': 'Vorname',
      'form.lastname': 'Nachname',
      'form.title_pre': 'Titel (vorangestellt)',
      'form.title_post': 'Titel (nachgestellt)',
      'form.email': 'E-Mail',
      'form.phone': 'Telefon',
      'form.service': 'Welche Leistung interessiert Sie?',
      'form.message': 'Ihre Frage oder Beschreibung',
      'form.submit': 'Anfrage senden →',
      'form.privacy': 'Mit dem Klick auf den Button stimmen Sie unserer Datenschutzerklärung zu.',
      'form.success': 'Gesendet ✓',
      'footer.services': 'Leistungen',
      'footer.company': 'Unternehmen',
      'footer.jurisdictions': 'Jurisdiktionen',
      'footer.shelf': 'Vorratsgesellschaften',
      'footer.industries': 'Branchen',
      'footer.consultation': 'Beratung',
      'footer.legal': 'Rechtliches',
      'footer.rights': '© 2024–2026 NEXORA Unternehmensberatung GmbH. Alle Rechte vorbehalten.',
      'page.services.title': 'Unsere Leistungen',
      'page.about.title': 'Über uns',
      'page.contact.title': 'Kontakt',
      'page.products.title': 'Produkte',
      'page.insights.title': 'Insights',
      // Page meta titles and descriptions
      'page.home.meta.title': 'NEXORA Unternehmensberatung — Unternehmensberatung in Österreich',
      'page.home.meta.desc': 'NEXORA — führende Beratungsgesellschaft in Wien für ukrainische Unternehmer. GmbH-Gründung, Konzernstrukturierung in 20+ Jurisdiktionen, Steuern, AML/Compliance.',
      'page.about.meta.title': 'Über uns — Team NEXORA Unternehmensberatung',
      'page.about.meta.desc': 'Das NEXORA-Team: Serhii Zaitsev MBA, Oleksandr Korniiets (Master of Law, IHK Mediator) und Lukas Hertig (Schweiz). Offiziell registriert in Österreich, FN 663874k.',
      'page.contact.meta.title': 'Kontakt — NEXORA Unternehmensberatung, Wien',
      'page.contact.meta.desc': 'Kontaktieren Sie NEXORA in Wien: Franz-Josefs-Kai 27/DG/9, 1010 Wien. Tel: +43 690 200 210 14.',
      'page.corporate.meta.title': 'Corporate Services — Firmengründung, Lizenzen, Vorratsgesellschaften | NEXORA',
      'page.corporate.meta.desc': 'Firmengründung in der EU und Offshore, Vorratsgesellschaften mit EMI/VASP/Forex-Lizenzen. 20+ Jurisdiktionen. Beratung aus Wien.',
      'page.insights.meta.title': 'Insights & Analysen — DORA, MiCA, AML, EU FinTech | NEXORA',
      'page.insights.meta.desc': 'Aktuelle Analysen zu DORA, MiCA, AML/KYC, PSD3 und regulatorischen Entwicklungen für Unternehmen in der EU.',
      'page.products.meta.title': 'Digitale Produkte — CRM, DORA, Compliance Tools | NEXORA',
      'page.products.meta.desc': 'Digitale Produkte von NEXORA für Unternehmen: CRM für Beratung, DORA-Compliance-Plattform, Jurisdictions Screener.',
      'page.services.meta.title': 'Leistungen — NEXORA Unternehmensberatung',
      'page.services.meta.desc': 'Sechs Beratungsbereiche in Österreich: LAUNCH, STRUCTURE, FINANCE & OPS, PEOPLE, SCALE, COMPLY.',
      'page.contact.eyebrow': 'Kontaktieren Sie uns',
      'page.contact.h1': 'Wir sind für Sie da — schreiben oder rufen Sie an',
      'page.contact.sub': 'Das erste Beratungsgespräch ist kostenlos. Wir antworten innerhalb von 24 Stunden an Werktagen.',
      'page.about.eyebrow': 'Unser Team',
      'page.about.h1': 'Ein Team, das zwei Welten versteht',
      'page.about.sub': 'NEXORA wurde von Ukrainern in Wien gegründet. Wir verstehen, wie österreichisches Business denkt — und wie unsere Kunden aus der Ukraine denken. Diese Brücke zwischen zwei Kulturen ist unser Hauptvorteil.',
      'page.about.story.eyebrow': 'Unsere Geschichte',
      'page.about.story.h2': 'Aus dem Bedarf geboren — gemeinsam mit Kunden wachsen',
      'page.about.cta.eyebrow': 'Bereit zu starten?',
      'page.services.h1': 'Sechs Bereiche. Ein Berater.',
      'page.services.sub': 'NEXORA bietet das gesamte Spektrum erlaubter Unternehmensberatungsleistungen — von der Unternehmensgründung bis zu Compliance und digitaler Transformation.',
      'page.services.cta.eyebrow': 'Bereit zu starten?',
      'page.products.h1': 'Digitales Werkzeug für ernsthaftes Business',
      'page.cta.eyebrow': 'Bereit zu starten?',
      'page.gruendung.meta.title': 'LAUNCH — Unternehmensgründung in Österreich | NEXORA',
      'page.gruendung.meta.desc': 'GmbH, FlexKapG, Einzelunternehmen in Österreich — von der Rechtsformwahl bis Firmenbuch und Gewerbeschein. Pakete ab €710.',
      'page.structure.meta.title': 'STRUCTURE — CFC, PE-Risiken, AT–UA Holdings | NEXORA',
      'page.structure.meta.desc': 'CFC/PE-Analyse, Holdingstrukturen, DBA AT–UA. Pakete SCAN €690 · SHIELD €1.490 · FULL €2.800.',
      'page.finance.meta.title': 'FINANCE & OPERATIONS — Geschäftskonto, Cashflow, KPI | NEXORA',
      'page.finance.meta.desc': 'Geschäftskonto-Eröffnung, KYC-Paket, Cashflow-Planung, KPI-Systeme, Management-Reporting.',
      'page.people.meta.title': 'PEOPLE & RESIDENCY — RWR-Karte, HR-Beratung in Österreich | NEXORA',
      'page.people.meta.desc': 'RWR-Karte für Unternehmer, Niederlassungsbewilligung, MA 35, HR-Beratung. ORIENTATION ab €490.',
      'page.comply.meta.title': 'COMPLY — AML/KYC, MiCA/CASP, DORA, Sanctions | NEXORA',
      'page.comply.meta.desc': 'AML/CFT-Programm, MiCA/CASP-Lizenzierung, DORA-Implementierung, Sanctions Screening für Unternehmen in der EU.',
      'page.scale.meta.title': 'SCALE & DIGITAL — DACH-Expansion, M&A, KI-Automatisierung | NEXORA',
      'page.scale.meta.desc': 'Market Entry DACH, M&A Advisory, Investitionsrunde, CRM/ERP-Beratung, KI-Automatisierung von Geschäftsprozessen.',
      // Leistungen subpage hero headings
      'page.gruendung.h1': 'LAUNCH — Unternehmensgründung in Österreich',
      'page.gruendung.sub': 'Von der Wahl der Rechtsform (GmbH / FlexKapG / Einzelunternehmen) bis zum Erhalt aller Registrierungsdokumente. Transparent, schnell, ohne unnötige Schritte.',
      'page.structure.h1': 'STRUCTURE — Konzernstrukturierung & grenzüberschreitende Beratung',
      'page.structure.sub': 'Holdingstrukturen AT–UA, Fragen zur Betriebsstätte und kontrollierten ausländischen Gesellschaften (CFC). DBA Österreich–Ukraine.',
      'page.finance.h1': 'FINANCE & OPERATIONS — Finanzen & operativer Support',
      'page.finance.sub': 'Geschäftskonto-Eröffnung, Cashflow-Planung, KPI-Systeme und Management-Reporting für Ihr österreichisches Unternehmen.',
      'page.people.h1': 'PEOPLE & RESIDENCY — Personal & Aufenthaltstitel',
      'page.people.sub': 'RWR-Karte für Unternehmer, Niederlassungsbewilligung, MA 35 und HR-Beratung für Ukrainer in Österreich.',
      'page.comply.h1': 'COMPLY — Compliance, AML/KYC & Regulatorische Beratung',
      'page.comply.sub': 'AML: FMA-Strafe bis zu €1 Mio. bei fehlendem Programm. MiCA: Betrieb ohne CASP-Lizenz ist illegal. DORA: IKT-Risiken — Meldepflicht.',
      'page.scale.h1': 'SCALE & DIGITAL — DACH-Markteintritt, Wachstum & digitale Transformation',
      'page.scale.sub': 'Österreichisches Unternehmen bereit für den DACH-Markt oder strategische Deals? NEXORA koordiniert M&A Advisory, Market Entry und Digital Growth.',
      // Aria-labels
      'aria.main.nav': 'Hauptnavigation',
      'aria.lang.switch': 'Sprache wechseln',
      'aria.lang.uk': 'Ukrainisch',
      'aria.lang.en': 'Englisch',
      'aria.lang.de': 'Deutsch',
      'aria.theme.switch': 'Design wechseln',
      'aria.menu.open': 'Menü öffnen',
      // About page (6a)
      'about.hero.h1': 'Unser Team',
      'about.hero.sub': 'Die Menschen, denen Sie Ihr Unternehmen anvertrauen',
      'about.values.title': 'Unsere Werte',
      'about.values.sub': 'Das Fundament jedes Projekts, das wir angehen',
      'about.val1.title': 'Zuverlässigkeit',
      'about.val1.text': 'Wir halten Fristen und Versprechen ein. Wenn wir "in 3 Wochen" gesagt haben — dann bedeutet das in 3 Wochen. Ihr Unternehmen kann nicht warten.',
      'about.val2.title': 'Ergebnis',
      'about.val2.text': 'Wir sind ergebnisorientiert, nicht prozessorientiert. Unser Ziel ist es, dass Ihr Unternehmen erfolgreich funktioniert und wächst.',
      'about.accred.title': 'Akkreditierung & Zulassung',
      'about.accred.sub': 'Offizielle Akkreditierung und Registrierung',
      'about.firmenbuch.text': 'Die Gesellschaft ist im Firmenbuch Österreich unter der FN 663874 k eingetragen. Volle rechtliche Transparenz und Rechtspersönlichkeit nach österreichischem Recht.',
      'about.uid.text': 'Eingetragene Mehrwertsteuerpflichtige in Österreich mit UID ATU82669239. Alle ausgestellten Rechnungen enthalten die Pflichtangaben gemäß österreichischem Recht.',
      'about.partners.title': 'Partner',
      'about.partners.sub': 'Unser Partnernetzwerk',
      'about.partners.text': 'Zur Erbringung umfassender Leistungen arbeitet NEXORA mit bewährten Partnern zusammen: österreichischen Notaren, Wirtschaftsprüfern und Rechtsanwaltskanzleien.',
      'about.partner1.title': 'Österreichische Notare',
      'about.partner1.desc': 'Firmengründung und Notarleistungen',
      'about.cta.title': 'Sprechen wir über Ihr Unternehmen',
      'about.cta.sub': 'Das Erstgespräch ist kostenlos. Wir melden uns innerhalb von 24 Stunden.',
      'about.cta.btn': 'Kontakt aufnehmen',
      'about.cta.btn2': 'Unsere Leistungen',
      // Contact page (6b)
      'contact.process.title': 'So funktioniert es',
      'contact.process.sub': 'Der Beratungsprozess',
      'contact.step1.num': '1',
      'contact.step1.title': 'Anfrage stellen',
      'contact.step1.text': 'Füllen Sie das Formular aus oder rufen Sie uns an. Wir antworten innerhalb von 24 Stunden an Werktagen.',
      'contact.step2.num': '2',
      'contact.step2.title': 'Kostenloses Erstgespräch',
      'contact.step2.text': '30-minütiges Erstgespräch — online oder in unserem Wiener Büro. Unverbindlich.',
      'contact.step3.num': '3',
      'contact.step3.title': 'Individuelles Angebot',
      'contact.step3.text': 'Nach dem Gespräch erhalten Sie ein konkretes Angebot mit Zeitplan und Kosten.',
      'contact.faq.title': 'FAQ',
      'contact.faq.sub': 'Schnelle Antworten',
      'contact.faq.q1': 'Ist das Erstgespräch wirklich kostenlos?',
      'contact.faq.a1': 'Ja. Das erste 30-minütige Beratungsgespräch ist immer kostenlos.',
      'contact.faq.q2': 'Finden Beratungen nur in Wien oder auch online statt?',
      'contact.faq.a2': 'Wir beraten sowohl in unserem Wiener Büro (Franz-Josefs-Kai 27) als auch online über Zoom oder MS Teams.',
      'contact.faq.q3': 'In welchen Sprachen finden Beratungen statt?',
      'contact.faq.a3': 'Wir beraten auf Ukrainisch, Deutsch und Englisch.',
      'contact.office.title': 'Unternehmensberatung und Corporate Services aus Wien',
      'contact.office.address': 'Franz-Josefs-Kai 27/DG/9, 1010 Wien, Österreich',
      // Corporate services (6c)
      'cs.disclaimer': 'Daten gemäß DSGVO geschützt. Wir geben sie nicht an Dritte weiter.',
      'cs.hero.h1': 'Internationale Corporate Structuring aus Wien',
      'cs.hero.sub': 'Firmenregistrierung, Vorratsgesellschaften mit Lizenzen, EMI/VASP/CASP-Lizenzierung und Bankkonten — vollständiger Rundum-Service. Echte Preise, geprüfte Regulierungsdaten.',
      'cs.hero.shelf.btn': 'Vorratsgesellschaften →',
      'cs.section.title': 'Was umfassen die Corporate Services',
      'cs.section.desc': 'Sechs Leistungsbereiche — von der Registrierung einer neuen Gesellschaft bis zum Verkauf einer Vorratsgesellschaft mit Lizenz und offenem Bankkonto.',
      'cs.shelf.title': 'Vorratsgesellschaften (Shelf)',
      'cs.shelf.desc': 'Eigenes Portfolio — ohne Broker. Wir registrieren und verwalten Gesellschaften in Schlüsseljurisdiktionen selbständig.',
      'cs.data.updated': 'Jurisdiktionsdaten aktualisiert April 2026. Beratungsleistungen — keine Rechtsberatung.',
      // Footer extras
      'footer.tagline': 'Unternehmensberatung und Corporate Services aus Wien',
      'footer.datenschutz': 'Datenschutz',
      'footer.cookies': 'Cookie-Richtlinie',
      'footer.haftung': 'Haftungsausschluss',
      // Insights page
      'ins.eyebrow': 'Insights & Analysen',
      'ins.h1.line1': 'Regulatorik, die',
      'ins.h1.em': 'wirklich zählt',
      'ins.sub': 'Praxisorientierte Analysen zu DORA, MiCA, AML/KYC, PSD3 und den wichtigsten regulatorischen Entwicklungen für Unternehmen in der EU.',
      'ins.stat1': 'Fachanalysen',
      'ins.stat3': 'Publikationssprachen', // updated
      'ins.filter.all': 'Alle Themen',
      'ins.filter.markt': 'Markteintritt',
      'ins.featured.badge': 'Neueste Analyse',
      'ins.art0.cat': 'Marktüberblick',
      'ins.art0.title': '2026: Regulatory Overhang als neue Normalität',
      'ins.art0.excerpt': 'Payments, Krypto und digitale Investments stehen 2026 vor einem dichten regulatorischen Umfeld. DORA, MiCA, PSD3 und CSRD schaffen gleichzeitig neue Pflichten für Unternehmen in der EU.',
      'ins.tag.regulatory': 'Regulatorik',
      'ins.tag.enforcement': 'Enforcement',
      'ins.read': 'Min. Lesezeit',
      'ins.read.full': 'Vollständig lesen →',
      'ins.grid.title': 'Alle Analysen',
      'ins.grid.sub': 'Aktuelle Fachbeiträge zu regulatorischen Entwicklungen in der EU',
      'ins.weiterlesen': 'Weiterlesen →',
      'ins.cat.market': 'Markteintritt',
      'ins.cat.regulatory': 'Regulatorik',
      'ins.cat.enforcement': 'Enforcement',
      'ins.cat.strategy': 'Compliance',
      'ins.cat.strat': 'Strategie',
      'ins.art1.title': 'DORA: Von klassischer IT-Security zur nachweisbaren Resilienz kritischer End-to-End-Services',
      'ins.art1.excerpt': 'Mit dem Digital Operational Resilience Act hat die EU einheitliche Standards für ICT-Risikomanagement eingeführt — inklusive IKT-Lieferkette und Drittanbieter-Management.',
      'ins.art2.title': 'Markteintritt in die EU 2026: MiCA und PSD3/PSR als neuer Regulierungsrahmen',
      'ins.art2.excerpt': 'Die EU tritt 2026 mit einem weitgehend abgeschlossenen Regelwerk für Krypto-Assets und Zahlungsverkehr an. Was das für Markteintrittsstrategien von FinTech- und Crypto-Unternehmen bedeutet.',
      'ins.art3.title': 'EBA integriert MiCA in ihr System von Risikoindikatoren',
      'ins.art3.excerpt': 'Am 28. Januar 2026 hat die EBA eine aktualisierte Liste von Risk Assessment Indicators veröffentlicht — und MiCA erstmals systematisch in die Aufsichtsanalytik integriert.',
      'ins.art4.title': 'MiCA ohne Illusionen: Wie EU-Aufseher Token-Projekte tatsächlich stoppen',
      'ins.art4.excerpt': 'Seit dem 30. Dezember 2024 ist MiCA für Emittenten von Krypto-Assets vollständig in Kraft. Was Aufsichtsbehörden tatsächlich tun und was das für DACH-Unternehmen bedeutet.',
      'ins.art5.title': 'MiCA Compliance Playbook: Was Krypto-Unternehmen jetzt konkret tun müssen',
      'ins.art5.excerpt': 'Ein strukturierter Überblick über die konkreten Schritte, die Krypto-Unternehmen zur Einhaltung der MiCA-Verordnung in der EU unternehmen müssen — mit Fokus auf DACH.',
      'ins.art6.title': 'Wachstumsstrategie für regulierte Unternehmen: DORA, MiCA und CSRD als Chance',
      'ins.art6.excerpt': 'Regulatorische Anforderungen wie DORA, MiCA und CSRD werden oft als Belastung gesehen. Dieser Beitrag zeigt, wie sie strategisch als Wettbewerbsvorteil genutzt werden können.',
      'ins.art7.title': 'MiCA Tokenomics & Governance: Was Emittenten wirklich beachten müssen',
      'ins.art7.excerpt': 'Die MiCA-Verordnung stellt klare Anforderungen an Tokenomics-Design und Governance-Strukturen. Dieser Beitrag erklärt, was Emittenten von Krypto-Assets konkret umsetzen müssen.',
      'ins.art8.title': 'Sanktions-Screening & PEP-Checks: Praxisleitfaden für Unternehmen in der EU',
      'ins.art8.excerpt': 'Sanktions-Screening und PEP-Checks sind für viele Unternehmen obligatorisch. Dieser Leitfaden erklärt die rechtlichen Anforderungen und zeigt, wie ein effektives Screening-System aufgebaut wird.',
      'ins.art9.title': 'AML/KYC Blueprint: Compliance-Framework zukunftssicher aufbauen',
      'ins.art9.excerpt': 'Ein strukturierter Blueprint für den Aufbau eines robusten AML/KYC-Compliance-Frameworks — angepasst an die aktuellen EU-Anforderungen und die Realitäten regulierter Unternehmen.',
      'ins.stat2.label': 'Abdeckungszeitraum',
      'ins.filter.aml': 'AML / KYC',
      'ins.filter.tax': 'Steuern',
      'ins.filter.launch': 'Markteintritt',
      'ins.filter.strategy': 'Strategie',
      'ins.a1.cat': 'Strategie / Überblick',
      'ins.a1.title': '2026: Regulatory Overhang als neue Normalität',
      'ins.a1.excerpt': 'Payments, Krypto-Assets und digitale Investments stehen 2026 vor einem dichten regulatorischen Umfeld. MiCA, DORA, AI Act, PSD3/PSR und Kapitalmarktreformen werden schrittweise umgesetzt — ein strategischer Überblick für Unternehmen in der EU.',
      'ins.a2.cat': 'DORA',
      'ins.a2.title': 'DORA: Von klassischer IT-Security zur nachweisbaren Resilienz kritischer End-to-End-Services',
      'ins.a2.excerpt': 'DORA bewertet nicht, ob ein Framework auf Papier existiert, sondern ob kritische Services tatsächlich unter Last weiterlaufen können — einschließlich der gesamten IKT-Lieferkette und Drittanbieter.',
      'ins.a3.cat': 'MiCA / FinTech',
      'ins.a3.title': 'Markteintritt EU 2026: MiCA und PSD3/PSR als neuer Regulierungsrahmen für FinTech und Crypto',
      'ins.a3.excerpt': '2026 wirkt als praktischer Filter: Ohne CASP-Lizenz können die meisten Krypto-Geschäftsmodelle in der EU nicht mehr betrieben werden. Österreich bleibt ein attraktiver Hub mit klarer FMA-Aufsicht und EU-Passporting.',
      'ins.a4.cat': 'MiCA / EBA',
      'ins.a4.title': 'EBA integriert MiCA in ihr Risikoindikatorensystem: Von der Randnotiz zum Kern der Aufsichtsanalytik',
      'ins.a4.excerpt': 'Am 28. Januar 2026 hat die EBA Krypto-Risiken erstmals in einen einheitlichen KRI-Satz aufgenommen — gleichrangig mit klassischen Bankenrisiken. Praktische Folgen für Banken, FinTech und CASP mit hybriden Modellen.',
      'ins.a5.cat': 'MiCA / Enforcement',
      'ins.a5.title': 'MiCA ohne Illusionen: Wie EU-Aufseher Token-Projekte tatsächlich stoppen — Fokus DACH',
      'ins.a5.excerpt': 'Seit dem 30. Dezember 2024 ist MiCA vollständig in Kraft. Nach Ablauf der Übergangsbestimmungen sind Regulatoren verpflichtet, nicht autorisierte Anbieter vom Markt zu nehmen. Was das für CASP im DACH-Raum bedeutet.',
      'ins.a6.cat': 'Strategie',
      'ins.a6.title': 'Wachstumsstrategie 2026–2027: Wie DORA, MiCA und CSRD in den Businessplan integriert werden',
      'ins.a6.excerpt': 'DORA, MiCA und CSRD sind keine reinen Compliance-Themen mehr — sie entscheiden über Marktzugang und Finanzierung. Ein Leitfaden zur Integration regulatorischer Anforderungen in die Wachstumsstrategie 2026–2027.',
      'ins.a7.cat': 'AML / Sanktionen',
      'ins.a7.title': 'Sanktions- und Compliance-Screening für KMU: Mindestkonfiguration zur Reduktion von EU-Sanktionsrisiken',
      'ins.a7.excerpt': 'EU-Sanktionen betreffen jedes KMU, das mit Drittländern oder sensiblen Gütern arbeitet. Vier zentrale Kontrollpunkte und eine Mindestkonfiguration ohne teure Software — ein praxisnaher Leitfaden.',
      'ins.a8.cat': 'AML / KYC',
      'ins.a8.title': 'AML/KYC Blueprint 2026: Von der Risikobewertung über Onboarding bis zur laufenden Überwachung (AMLR & 6AMLD)',
      'ins.a8.excerpt': 'Ein vollständiger End-to-End AML/KYC-Prozess für FinTech, Kryptoanbieter und Zahlungssysteme: Risikobewertungsvorlagen, Entscheidungsbäume für Onboarding, Überwachungsleitfaden und Governance-Rollen.',
      'ins.a9.cat': 'MiCA / Tech',
      'ins.a9.title': 'MiCA Compliance Playbook für Produkt- und Technologieteams',
      'ins.a9.excerpt': 'MiCA ist kein reines Rechtsthema. Für Produktmanager und CTOs bedeutet die Verordnung konkrete Anforderungen an Systeme, Datenarchitektur und IT-Governance. Ein Playbook für technische Teams im DACH-Raum.',
      'ins.a10.cat': 'MiCA / Tokenomics',
      'ins.a10.title': 'MiCA-konforme Tokenomics und Governance für EU-Kryptoprojekte (Fokus: DACH-Region)',
      'ins.a10.excerpt': 'MiCA verlangt nicht nur transparente Dokumentation, sondern auch wirtschaftlich integres Tokenomics-Design. Interessenkonfliktmanagement, Governance-Strukturen und White-Paper-Anforderungen für CASP und Emittenten.',
      'ins.a11.cat': 'MiCA / Investoren',
      'ins.a11.title': 'MiCA/CASP Investor-Briefing — Risiken und Chancen',
      'ins.a11.excerpt': 'Für Investoren, die Beteiligungen an EU-CASP oder Token-Projekten prüfen: Wie MiCA die regulatorische Landschaft verändert, was ein Lizenzstatus bedeutet und welche Red Flags zu beachten sind.',
      'ins.a12.cat': 'MiCA / Österreich',
      'ins.a12.title': 'MiCA in Österreich: Roadmap zur CASP-Lizenz für Kryptoprojekte',
      'ins.a12.excerpt': 'Eine praxisnahe Roadmap für Ende 2025 bis Anfang 2026: Dokumentationschecklisten, typische Fehler, FMA-Lizenzzeitplan und wann ein Partnermodell ausreicht statt einer eigenen Lizenz.',
      'ins.a13.cat': 'DORA / Österreich',
      'ins.a13.title': 'DORA in Österreich/EU: Wer fällt darunter und 10 Schritte zur Mindest-Compliance',
      'ins.a13.excerpt': 'DORA gilt seit dem 17. Januar 2025 für nahezu alle beaufsichtigten Finanzinstitute in der EU. Dieser Beitrag erklärt, wer unter die Verordnung fällt, und bietet 10 konkrete Schritte zur Mindest-Compliance.',
      'ins.a14.cat': 'Steuern / Lohn',
      'ins.a14.title': 'Steuern und Lohnnebenkosten in Österreich 2026: Gründerleitfaden für die ersten 90 Tage',
      'ins.a14.excerpt': 'Die ersten 90 Tage nach der GmbH-Gründung in Österreich sind steuerlich entscheidend. Körperschaftsteuer, USt, Lohnnebenkosten und ein realistischer 90-Tage-Aktionsplan für internationale Gründer.',
      'ins.a15.cat': 'Gründung / KYC',
      'ins.a15.title': 'Bankkonto für ausländische Gründer in Österreich 2026: KYC-Checkliste, UBO-Nachweis und Business Case',
      'ins.a15.excerpt': 'Die Kontoeröffnung für eine GmbH in Österreich ist der kritischste Schritt — aufwändiger als die Firmenbuch-Eintragung selbst. KYC-Checkliste, UBO-Nachweis und Business-Case-Vorbereitung für die Bank.',
      'ins.a16.cat': 'Gründung / Österreich',
      'ins.a16.title': 'Unternehmensgründung und Behördenkommunikation in Österreich 2026: Firmenbuch, Gewerbe, UID',
      'ins.a16.excerpt': 'Firmenbuch, Gewerbebehörde und Finanzamt arbeiten sequenziell — jeder Fehler verzögert die Eintragung um Wochen. Schritt-für-Schritt-Verfahren, Dokumente und typische Behördenanfragen.',
      'ins.a17.cat': 'Gründung / Struktur',
      'ins.a17.title': 'GmbH gegen EPU gegen OG: Welche Rechtsform eignet sich wirklich für internationale Gründer?',
      'ins.a17.excerpt': 'Die Wahl der Rechtsform in Österreich beeinflusst persönliche Haftung, Finanzierungsmöglichkeiten und Investorenattraktivität. Entscheidungsbaum und fünf reale Geschäftsfälle für internationale Gründer.',
      'ins.a18.cat': 'Markteintritt',
      'ins.a18.title': 'Markteintritt Österreich 2025–2026: Schritt-für-Schritt-Anleitung für internationale Gründer',
      'ins.a18.excerpt': 'Wien ist das Tor zum DACH-Markt mit über 100 Millionen Verbrauchern. Schritt-für-Schritt-Anleitung von der Rechtsformwahl bis zur Kontoeröffnung — mit Checklisten und lokalen Partnern.',
      'ins.lm.h2': 'Regulatorik-Briefing',
      'ins.lm.h2em': 'direkt in Ihr Postfach',
      'ins.lm.desc': 'Monatliche Zusammenfassung der wichtigsten regulatorischen Entwicklungen — kompakt, praxisnah und ohne Fachjargon. Für Entscheider, die auf dem Laufenden bleiben müssen.',
      'ins.lm.perk1': 'DORA, MiCA, AML/KYC, PSD3 — monatlich kuratiert',
      'ins.lm.perk2': 'Praxistipps und Handlungsempfehlungen',
      'ins.lm.perk3': 'Kostenlos, jederzeit kündbar',
      'ins.lm.formtitle': 'Jetzt anmelden',
      'ins.lm.formhint': 'Für Unternehmen und Fachleute in der EU',
      'ins.lm.name': 'Ihr Name',
      'ins.lm.email': 'Geschäftliche E-Mail-Adresse',
      'ins.lm.size': 'Unternehmensgröße',
      'ins.lm.s1': '1–10 Mitarbeiter',
      'ins.lm.s2': '11–50 Mitarbeiter',
      'ins.lm.s3': '51–200 Mitarbeiter',
      'ins.lm.s4': '200+ Mitarbeiter',
      'ins.lm.btn': 'Briefing abonnieren →',
      'dok.h1': 'Vertragsvorlagen & Rechtsdokumente',
      'dok.sub': 'Professionelle Vorlagen fuer Ihre Geschaeftsbeziehungen — erstellt nach oesterreichischem und EU-Recht. Alle Dokumente werden individuell angepasst und sind nicht als Rechtsberatung zu verstehen.',
      'dok.grid.title': 'Verfuegbare Vorlagen',
      'dok.grid.sub': 'Alle Vorlagen entsprechen dem Stand April 2026 und sind nach oesterreichischem Recht sowie DSGVO/EU-Verordnungen ausgerichtet.',
      'dok.doc1.title': 'Beratungsvertrag',
      'dok.doc1.desc': 'Standardvertrag fuer Beratungsleistungen zwischen NEXORA und Auftraggebern. Regelt Leistungsumfang, Honorar, Vertraulichkeit, Haftung und Kuendigung nach oesterreichischem Recht (ABGB, UGB).',
      'dok.doc2.title': 'Geheimhaltungsvereinbarung (NDA)',
      'dok.doc2.desc': 'Non-Disclosure Agreement fuer den Schutz vertraulicher Informationen in Beratungsprojekten, Due-Diligence und M&A-Transaktionen. Wechselseitig oder einseitig konfigurierbar.',
      'dok.doc3.title': 'Auftragsverarbeitungsvertrag (AVV)',
      'dok.doc3.desc': 'Datenschutzrechtlich erforderlicher Vertrag gemaess Art. 28 DSGVO. Enthaelt alle Pflichtangaben laut DSGVO sowie technisch-organisatorische Massnahmen (TOMs).',
      'dok.doc4.title': 'Datenschutzerklaerung',
      'dok.doc4.desc': 'Vollstaendige Datenschutzerklaerung gemaess DSGVO fuer Website-Betreiber und Unternehmen. Regelt Rechtsgrundlagen, Betroffenenrechte, Cookie-Management und internationale Datenuebermittlung.',
      'dok.doc5.title': 'Allgemeine Geschaeftsbedingungen (AGB)',
      'dok.doc5.desc': 'Vollstaendige AGB fuer Beratungsunternehmen nach oesterreichischem Recht. Umfasst Leistungsgegenstand, Honorare, Haftung, Vertraulichkeit, Datenschutz, Kuendigung und Konsumentenschutz.',
      'dok.doc6.title': 'Projektvereinbarung & Angebot',
      'dok.doc6.desc': 'Vorlage fuer projektspezifische Vereinbarungen mit Leistungsbeschreibung, Zeitplan, Meilensteinen und Zahlungsplan. Kommt bald.',
      'dok.disclaimer.title': 'Wichtiger Hinweis',
      'dok.disclaimer.text': 'Diese Vorlagen dienen als Ausgangspunkt und ersetzen keine individuelle Rechtsberatung. NEXORA empfiehlt, alle Dokumente vor Unterzeichnung durch einen Rechtsanwalt pruefen zu lassen.',
      'dok.cta.title': 'Individuelle Vertragsgestaltung',
      'dok.cta.desc': 'Benoetigen Sie massgeschneiderte Vertraege? Unser Rechtsteam erstellt individuelle Dokumente fuer M&A-Transaktionen, internationale Strukturierungen und regulatorische Anforderungen.',
      'ins.lm.disclaimer': 'Kein Spam. Keine Weitergabe an Dritte. Datenschutz gemäß DSGVO.',
      'hero.eyebrow': 'Business Consulting in Wien',
      'hero.h1': 'Ihr zuverlässiger|Geschäftspartner|in Österreich',
      'hero.stat1': 'Leistungsbereiche',
      'hero.stat2': 'Marktpräsenz',
      'hero.stat3.sub': 'MiCA · AML · PSD3',
      'section.services.h2': 'Unterstützung in jeder Phase Ihres Unternehmens',
      'svc.launch.h3': 'Unternehmensgründung & Start',
      'svc.launch.p': 'GmbH, FlexKapG, Einzelunternehmen — Rechtsformwahl, Dokumente, Firmenbuch, Gewerbeanmeldung. Koordination mit Notar und Steuerberater.',
      'svc.structure.h3': 'Konzernstrukturierung',
      'svc.structure.p': 'CFC/KIK-Analyse, PE-Risikobewertung, AT–UA Holdingstrukturen, DBA-Analyse. Strategie für internationale Unternehmen.',
      'svc.finance.h3': 'Finanzielle Transparenz',
      'svc.finance.p': 'Cashflow-Planung, KPI-Systeme, Management-Reporting, Unternehmensbewertung, Sanierungsberatung — Management-Analyse für Ihr Unternehmen.',
      'svc.people.h3': 'Team & Aufenthaltsstatus',
      'svc.people.p': 'HR-Beratung, Rot-Weiß-Rot-Karte, Niederlassungsbewilligung, Beschäftigungsformen, ÖGK. Für Unternehmer und ihre Teams in Österreich.',
      'svc.scale.h3': 'Digital Growth & Scale-up',
      'svc.scale.p': 'Strategisches Advisory, EU/DACH-Markteintritt, M&A-Begleitung, Partnersuche, digitale Transformation von Betriebsprozessen.',
      'svc.comply.h3': 'Compliance & AML',
      'svc.comply.p': 'DORA, MiCA, AML/KYC, PSD3 — regulatorische Begleitung für FinTech- und Krypto-Unternehmen. Aufbau von Compliance-Frameworks.',
      'cta.more': 'Mehr erfahren →',
      // WHY NEXORA
      'why1.title': 'Lizenziertes Unternehmen in Österreich',
      'why1.text': 'NEXORA Unternehmensberatung GmbH ist im Firmenbuch eingetragen (FN 663874k), besitzt Gewerbeberechtigung GISA Nr.38919244 für „Unternehmensberatung einschließlich der Unternehmensorganisation" und die bestätigte UID ATU82669239.',
      'why2.title': 'Wir kennen beide Systeme von innen',
      'why2.text': 'Unser Team verfügt über praktische Erfahrung im österreichischen und ukrainischen Recht. Wir verstehen die Besonderheiten des post-sowjetischen Geschäftslebens, des ukrainischen Steuerrechts und des österreichischen Gesellschaftsrechts gleichzeitig.',
      'why3.title': 'Internationales Partnernetzwerk',
      'why3.text': 'Über vertrauenswürdige Partner haben wir Zugang zu 20+ Jurisdiktionen: EU, Dubai, Singapur, Schweiz und weitere. Wir koordinieren, wenn spezialisierte lizenzierte Unterstützung benötigt wird.',
      'why4.title': 'Vollzyklus — von der Gründung bis zur Skalierung',
      'why4.text': 'LAUNCH → ACCOUNTS → STRUCTURE → RESIDENCY → SCALE → COMPLY. Sie wechseln nicht von Berater zu Berater — den gesamten Weg mit einem Partner.',
      'why5.title': 'Konformität mit österreichischem Recht',
      'why5.text': 'Wir handeln strikt innerhalb des Berufsbilds Unternehmensberatung (§136 GewO). Wo ein Steuerberater oder Rechtsanwalt erforderlich ist, kommunizieren wir das offen und koordinieren bewährte Partner.',
      'why6.title': 'Vertraulichkeit und NDA-Ansatz',
      'why6.text': 'Vor jedem Austausch sensibler Informationen unterzeichnen wir eine NDA. Besonders kritisch bei Strukturfragen, KFC-Themen und Defence/Regulated Business.',
      // AUDIENCE
      'aud1.title': 'Ukrainer in Österreich',
      'aud1.text': 'Unternehmer und Fachleute, die nach Österreich umgezogen sind und ihr Unternehmen eröffnen oder verlagern möchten. Wir sprechen Ihre Muttersprache.',
      'aud2.title': 'IT / Tech-Unternehmen',
      'aud2.text': 'Technologieunternehmen, die eine zuverlässige EU-Jurisdiktion für die Skalierung suchen. Wir verstehen die Besonderheiten des IT-Geschäfts.',
      'aud3.title': 'FinTech / Crypto / FOREX',
      'aud3.text': 'Unternehmen im Finanzbereich, die EMI-, VASP- oder FOREX-Lizenzen benötigen. Unterstützung beim MiCA-Deadline 2026.',
      'aud4.title': 'E-Commerce',
      'aud4.text': 'Online-Shops und Marktplätze, die eine österreichische oder EU-Rechtsadresse für den europäischen Markt erwerben möchten.',
      'aud5.title': 'Internationale Unternehmer',
      'aud5.text': 'Geschäftsleute aus anderen Ländern, die Österreich als Plattform für den EU-Markteintritt wählen. Wir helfen von Grund auf.',
      'aud6.title': 'Defence / MilTech',
      'aud6.text': 'Unternehmen im Verteidigungs- und Technologiesektor, die Konzernstrukturierung und internationales Compliance benötigen.',
      // PROCESS
      'proc1.title': 'Beratung',
      'proc1.text': 'Kostenloses Erstgespräch. Wir besprechen Ihre Ziele und Situation. Erstes Kennenlernen.',
      'proc2.title': 'Situationsanalyse',
      'proc2.text': 'Wir analysieren Ihre Bedürfnisse, erarbeiten die optimale Strategie und den Aktionsplan. Wir empfehlen die beste Lösung.',
      'proc3.title': 'Umsetzung',
      'proc3.text': 'Wir setzen den Plan um. Wir begleiten Sie auf jedem Schritt — von den Dokumenten bis zur Registrierung.',
      'proc4.title': 'Support',
      'proc4.text': 'Nach Projektabschluss bleiben wir bei Bedarf erreichbar. Laufende Unterstützung in Koordination mit lizenzierten Partnern.',
      // MiCA BANNER
      'mica.eyebrow': '⚠️ Dringender Termin',
      'mica.title': 'MiCA-Deadline: 1. Juli 2026 — Weniger als 3 Monate verbleiben',
      'mica.sub': 'Wenn Ihr Unternehmen mit Krypto-Assets in der EU tätig ist, müssen Sie bis zum 1. Juli 2026 eine VASP-Lizenz oder MiCA-Autorisierung erhalten. Wir helfen Ihnen, rechtzeitig fertig zu werden.',
      'mica.cta': 'Mehr über Corporate Services erfahren →',
      // REVIEWS
      'reviews.eyebrow': 'Referenzen',
      'reviews.title': 'Was unsere Kunden sagen',
      'rev1.text': '"NEXORA hat uns geholfen, eine GmbH in Wien in nur drei Wochen zu gründen. Kommunikation nur auf Ukrainisch — keinerlei Barriere. Serhii erklärte jeden Schritt, wir wussten, was auf jeder Etappe passiert."',
      'rev1.name': 'Oleg Martynenko',
      'rev1.role': 'IT-Unternehmer, Wien',
      'rev2.text': '"Unser E-Commerce-Unternehmen brauchte eine Rechtsadresse in der EU. NEXORA strukturierte alles optimal — mit minimaler Steuerlast und vollständiger Compliance. Ich empfehle Oleksandr sehr für den Rechtsteil."',
      'rev2.name': 'Natalia Kovalenko',
      'rev2.role': 'E-Commerce, Gründerin',
      'rev3.text': '"Wir sind im FinTech-Bereich tätig und brauchten eine VASP-Lizenz in der EU. Das NEXORA-Team kannte alle Feinheiten des Regulierungsprozesses. Dank ihrer Arbeit haben wir die Lizenz rechtzeitig und ohne unnötigen Stress erhalten."',
      'rev3.name': 'Vasyl Bondarenko',
      'rev3.role': 'FinTech CEO, Wien',
      // CONTACT CTA
      'section.cta.eyebrow': 'Starten wir gemeinsam',
      'contact.phone.label': 'Telefon',
      'contact.email.label': 'E-Mail',
      // COOKIE BANNER
      'cookie.title': 'Wir verwenden Cookies',
      'cookie.body': 'Wir verwenden Cookies zur Verbesserung der Website und zur Analyse des Traffics. Unsere <a href="cookies/index.html">Cookie-Richtlinie</a> ansehen.',
      'cookie.reject': 'Nur notwendige',
      'cookie.accept': 'Alle akzeptieren',
    }
  };

  function applyLang(lang) {
    var t = TRANSLATIONS[lang] || TRANSLATIONS['uk'];
    // Keys that need innerHTML (contain HTML tags in translations)
    var htmlKeys = ['hero.h1'];
    htmlKeys.forEach(function(key) {
      var val = t[key];
      if (!val) return;
      document.querySelectorAll('[data-i18n="' + key + '"]').forEach(function(el) {
        // For hero h1, reconstruct with accent span
        if (key === 'hero.h1') {
          var parts = val.split('|');
          if (parts.length === 3) {
            el.innerHTML = parts[0] + '<br/><span class="accent">' + parts[1] + '</span><br/>' + parts[2];
          } else {
            el.textContent = val;
          }
        }
      });
    });
    // Update all data-i18n elements (skip htmlKeys already handled above)
    document.querySelectorAll('[data-i18n]').forEach(function(el) {
      var key = el.getAttribute('data-i18n');
      if (htmlKeys.indexOf(key) !== -1) return;
      if (t[key] !== undefined) el.textContent = t[key];
    });
    // Update html lang attribute
    document.documentElement.lang = lang === 'uk' ? 'uk' : lang === 'de' ? 'de' : 'en';
    // Mark active button
    document.querySelectorAll('.lang-btn').forEach(function(btn) {
      btn.classList.toggle('active', btn.getAttribute('data-lang') === lang);
    });
    // Update form submit button if form exists
    var submitBtn = document.querySelector('#contactForm [type=submit]');
    if (submitBtn && !submitBtn.disabled) {
      if (t['form.submit']) submitBtn.textContent = t['form.submit'];
    }
    // Update page title and meta description based on page-specific keys
    var pageId = (document.querySelector('meta[name="nx-page-id"]') || {}).content || '';
    if (pageId && t['page.' + pageId + '.meta.title']) {
        document.title = t['page.' + pageId + '.meta.title'];
    }
    if (pageId) {
        var metaDesc = document.querySelector('meta[name="description"]');
        if (metaDesc && t['page.' + pageId + '.meta.desc']) {
            metaDesc.setAttribute('content', t['page.' + pageId + '.meta.desc']);
        }
    }

    // Update aria-labels for navigation elements
    var ariaMap = {
      'aria-nav': t['aria.main.nav'] || 'Navigation',
      'aria-lang': t['aria.lang.switch'] || 'Switch language',
      'aria-theme': t['aria.theme.switch'] || 'Switch theme',
      'aria-menu': t['aria.menu.open'] || 'Open menu'
    };
    var navEl = document.querySelector('nav[aria-label]');
    if (navEl && t['aria.main.nav']) navEl.setAttribute('aria-label', t['aria.main.nav']);
    var langBtn = document.querySelector('.lang-switcher, [aria-label*="мов"], [aria-label*="lang"], [aria-label*="Sprach"]');
    if (langBtn && t['aria.lang.switch']) langBtn.setAttribute('aria-label', t['aria.lang.switch']);
    var themeBtn = document.querySelector('[aria-label*="тем"], [aria-label*="theme"], [aria-label*="Design"]');
    if (themeBtn && t['aria.theme.switch']) themeBtn.setAttribute('aria-label', t['aria.theme.switch']);
    var menuBtn = document.querySelector('[aria-label*="меню"], [aria-label*="menu"], [aria-label*="Menü"]');
    if (menuBtn && t['aria.menu.open']) menuBtn.setAttribute('aria-label', t['aria.menu.open']);
    // Persist
    try { localStorage.setItem('nx-lang', lang); } catch(e){}
    // Update all internal nav links to carry lang param
    if (lang !== 'uk') {
        document.querySelectorAll('a[href]').forEach(function(a) {
            var href = a.getAttribute('href');
            if (href && (href.startsWith('./') || href.startsWith('../') || href.startsWith('index')) && !href.includes('?lang=') && !href.startsWith('#') && !href.startsWith('mailto') && !href.startsWith('tel') && !href.startsWith('http')) {
                a.setAttribute('href', href + (href.includes('?') ? '&' : '?') + 'lang=' + lang);
            }
        });
    }
  }

  function init() {
    var saved = 'uk';
    // Check URL parameter first (allows lang to persist via links)
    try {
        var urlParams = new URLSearchParams(window.location.search);
        var urlLang = urlParams.get('lang');
        if (urlLang && ['uk', 'en', 'de'].indexOf(urlLang) !== -1) {
            saved = urlLang;
            localStorage.setItem('nx-lang', saved);
        } else {
            saved = localStorage.getItem('nx-lang') || 'uk';
        }
    } catch(e) {
        try { saved = localStorage.getItem('nx-lang') || 'uk'; } catch(e2){}
    }
    applyLang(saved);
    document.querySelectorAll('.lang-btn').forEach(function(btn) {
      btn.addEventListener('click', function() {
        applyLang(btn.getAttribute('data-lang'));
      });
    });
  }

  if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', init);
  } else {
    init();
  }
})();

// ============================================================
// Full DOM Text Replacement (for non-data-i18n text nodes)
// ============================================================
(function() {
  function applyTextMap(lang) {
    if (!window.NX_TEXT_MAP) return;
    if (!document.body) return;
    var map = window.NX_TEXT_MAP;
    // Walk all text nodes
    var walker = document.createTreeWalker(
      document.body, NodeFilter.SHOW_TEXT,
      { acceptNode: function(node) {
        var p = node.parentNode;
        if (!p) return NodeFilter.FILTER_REJECT;
        var tag = (p.tagName||'').toLowerCase();
        if (['script','style','noscript'].indexOf(tag) !== -1) return NodeFilter.FILTER_REJECT;
        return NodeFilter.FILTER_ACCEPT;
      }}
    );
    var node, nodes = [];
    while ((node = walker.nextNode())) nodes.push(node);
    nodes.forEach(function(node) {
      if (!node.parentNode) return;
      var orig = node._nxOriginal !== undefined ? node._nxOriginal : null;
      var current = node.nodeValue;
      var trimmed = (orig || current).trim();
      if (!trimmed || trimmed.length < 2) return;
      // Store original UK text on first encounter
      if (node._nxOriginal === undefined) node._nxOriginal = current;
      if (lang === 'uk') {
        node.nodeValue = node._nxOriginal;
      } else {
        var entry = map[node._nxOriginal.trim()];
        if (entry && entry[lang]) {
          var lead = node._nxOriginal.match(/^\s*/)[0];
          var trail = node._nxOriginal.match(/\s*$/)[0];
          node.nodeValue = lead + entry[lang] + trail;
        }
      }
    });
  }
  window.NX_applyTextMap = applyTextMap;
  // Hook into lang buttons
  document.addEventListener('DOMContentLoaded', function() {
    document.querySelectorAll('.lang-btn').forEach(function(btn) {
      btn.addEventListener('click', function() {
        setTimeout(function() { applyTextMap(btn.getAttribute('data-lang')); }, 10);
      });
    });
    // Apply on load
    var lang = 'uk';
    try {
      var p = new URLSearchParams(window.location.search).get('lang');
      if (p && ['uk','en','de'].indexOf(p) !== -1) lang = p;
      else lang = localStorage.getItem('nx-lang') || 'uk';
    } catch(e) {}
    if (lang !== 'uk') setTimeout(function() { applyTextMap(lang); }, 50);
  });
})();

// ============================================================
// Hero Background Switcher — головна сторінка
// ============================================================
(function() {
  var hero = document.getElementById('mainHero');
  var btns = document.querySelectorAll('.hero-bg-btn');
  if (!hero || !btns.length) return;

  var BG_CLASSES = ['bg-wien', 'bg-kyiv', 'bg-dark'];

  // Restore saved choice
  var saved = 'wien';
  try { saved = sessionStorage.getItem('nx-hero-bg') || 'wien'; } catch(e) {}
  setHeroBg(saved);

  btns.forEach(function(btn) {
    btn.addEventListener('click', function() {
      setHeroBg(btn.getAttribute('data-bg'));
    });
  });

  function setHeroBg(bg) {
    BG_CLASSES.forEach(function(c) { hero.classList.remove(c); });
    hero.classList.add('bg-' + bg);
    btns.forEach(function(b) {
      b.classList.toggle('active', b.getAttribute('data-bg') === bg);
    });
    try { sessionStorage.setItem('nx-hero-bg', bg); } catch(e) {}
  }
})();

// ============================================================
// Cookie Consent Banner
// ============================================================
(function() {
  var CONSENT_KEY = 'nx-cookie-consent';

  function nxCookieSet(type) {
    try { localStorage.setItem(CONSENT_KEY, type); } catch(e){}
    var banner = document.getElementById('nx-cookie-banner');
    if (banner) {
      banner.classList.remove('nx-cookie-visible');
      setTimeout(function() { if (banner.parentNode) banner.parentNode.removeChild(banner); }, 400);
    }
  }

  // Make nxCookieSet global so onclick attributes work
  window.nxCookieSet = nxCookieSet;

  function injectBanner() {
    // Determine current language
    var lang = 'uk';
    try { lang = localStorage.getItem('nx-lang') || 'uk'; } catch(e){}

    var texts = {
      uk: {
        title: '🍪 Ми використовуємо cookies',
        body: 'Ми використовуємо технічно необхідні cookies для роботи сайту та аналізу трафіку. Детальніше у нашій',
        link: 'Політика Cookies',
        essential: 'Лише необхідні',
        accept: 'Прийняти все'
      },
      de: {
        title: '🍪 Wir verwenden Cookies',
        body: 'Wir verwenden technisch notwendige Cookies für den Betrieb der Website und zur Analyse des Datenverkehrs. Mehr in unserer',
        link: 'Cookie-Richtlinie',
        essential: 'Nur notwendige',
        accept: 'Alle akzeptieren'
      },
      en: {
        title: '🍪 We use cookies',
        body: 'We use technically necessary cookies to operate the website and analyze traffic. More in our',
        link: 'Cookie Policy',
        essential: 'Essential only',
        accept: 'Accept all'
      }
    };

    var t = texts[lang] || texts['uk'];

    // Detect path to cookies page using embedded depth meta tag
    var depthMeta = document.querySelector('meta[name="nx-depth"]');
    var depth = depthMeta ? parseInt(depthMeta.getAttribute('content'), 10) : 0;
    var cookieHref = (depth === 0 ? '.' : Array(depth).fill('..').join('/')) + '/cookies/index.html';

    var banner = document.createElement('div');
    banner.id = 'nx-cookie-banner';
    banner.setAttribute('role', 'dialog');
    banner.setAttribute('aria-label', 'Cookie налаштування');
    banner.innerHTML =
      '<div class="nx-cookie-inner">' +
        '<div class="nx-cookie-text">' +
          '<h4>' + t.title + '</h4>' +
          '<p>' + t.body + ' <a href="' + cookieHref + '">' + t.link + '</a>.</p>' +
        '</div>' +
        '<div class="nx-cookie-actions">' +
          '<button class="nx-cookie-essential" onclick="nxCookieSet(\'essential\')">' + t.essential + '</button>' +
          '<button class="nx-cookie-accept" onclick="nxCookieSet(\'all\')">' + t.accept + '</button>' +
        '</div>' +
      '</div>';

    document.body.appendChild(banner);

    // Show with delay
    setTimeout(function() {
      banner.classList.add('nx-cookie-visible');
    }, 600);
  }

  function initCookieBanner() {
    var consent = null;
    try { consent = localStorage.getItem(CONSENT_KEY); } catch(e){}
    if (!consent) {
      if (document.readyState === 'loading') {
        document.addEventListener('DOMContentLoaded', injectBanner);
      } else {
        injectBanner();
      }
    }
  }

  initCookieBanner();
})();
