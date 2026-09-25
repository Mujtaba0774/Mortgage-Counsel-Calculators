# Mortgage Counsel — Calculators

![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black) ![HTML](https://img.shields.io/badge/HTML-E34F26?style=for-the-badge&logo=html5&logoColor=white) ![CSS](https://img.shields.io/badge/CSS-1572B6?style=for-the-badge&logo=css&logoColor=white) ![SVG](https://img.shields.io/badge/SVG-FFB13B?style=for-the-badge&logo=svg&logoColor=black) ![Webflow](https://img.shields.io/badge/Webflow-4353FF?style=for-the-badge&logo=webflow&logoColor=white) ![Make.com](https://img.shields.io/badge/Make.com-6D00CC?style=for-the-badge&logo=make&logoColor=white) ![Zoho CRM](https://img.shields.io/badge/Zoho%20CRM-E42527?style=for-the-badge&logo=zoho&logoColor=white)

Seven home-loan calculators built by **Forrentech** for the **Mortgage Counsel** website. Each calculator is a single self-contained HTML file (markup, CSS and JavaScript together, no build step). They are pasted into **Webflow Embed** elements on the live site:

**Live site:** https://mortgagecounsel.webflow.io/calculators

![Calculators page](https://mujtabaasif.vercel.app/assets/projects-screenshots/mortgage-calculators/calculators-page.webp)

**Portfolio:** https://mujtabawd.vercel.app/

## The calculators

| Webflow page | Calculator | What it answers |
| --- | --- | --- |
| [/stamp-duty-calculator](https://mortgagecounsel.webflow.io/stamp-duty-calculator) | Stamp Duty | Stamp duty, mortgage and transfer fees, and grants for all 8 states and territories |
| [/borrowing-power-calculator](https://mortgagecounsel.webflow.io/borrowing-power-calculator) | Borrowing Power | Maximum loan a lender may approve, based on income, tax, living costs (HEM) and debts |
| [/repayment-calculator](https://mortgagecounsel.webflow.io/repayment-calculator) | Extra Repayment | Time and interest saved by paying extra each period |
| [/refinance](https://mortgagecounsel.webflow.io/refinance) | Loan Comparison | Whether a new loan (intro rate + fees) beats the existing one over the term |
| [/home-loan-balance](https://mortgagecounsel.webflow.io/home-loan-balance) | Loan Repayment | Repayment per period, total interest and fees, balance over time |
| [/rent-to-buy](https://mortgagecounsel.webflow.io/rent-to-buy) | Rent to Buy | Loan size that current monthly rent could repay |
| [/deposit-time-line-calculator](https://mortgagecounsel.webflow.io/deposit-time-line-calculator) | Deposit Timeline | How long to reach 5%, 10%, 15% and 20% deposits, with estimated LMI at each |

Every calculator also has a **Contact us** form (Send) and a **Book a call** calendar. Both send leads to Make.com (see [Lead capture](#lead-capture-makecom)).

## Which file is live

The folder holds working copies and drafts, and the file names don't say which is live. This table was checked against the live Webflow pages on 24 September 2026.

| Webflow page | Local source file | In sync with Webflow? |
| --- | --- | --- |
| Stamp Duty | `stamp-duty-calculator (1) copy.html` | Yes |
| Borrowing Power | `borrowingold.html` | Yes. `borrowing-calculater.html` is newer and **not uploaded yet** (it scrolls to the page heading instead of the progress dots between steps) |
| Extra Repayment | Markup: `Extra-Repayments-Calculator(Repayments Calculator) copy 2.html`, script: `...copy.html` | **No — Webflow is newer.** The live version only attaches results to a lead after *Calculate* is pressed |
| Loan Comparison | `Refinance.html` | **No — Webflow is newer** (same change as above) |
| Loan Repayment | `Repayment-calculator(Home Loan Balance : Amortization) copy.html` | Yes, apart from small differences |
| Rent to Buy | `rent-to-buy-calculator(Rent_To_Buy) copy.html` | Yes |
| Deposit Timeline | `Deposit-Time-Line-Calculator copy.html` | Yes |
| Footer form on every page | `ctafooter.html` | Yes |
| Contact Us page form | `cta_main.html` | Yes |

Before editing Extra Repayment or Loan Comparison, copy the current code out of Webflow first. Otherwise the live fixes will be lost.

**Other files**

| File | What it is |
| --- | --- |
| `cta_home.html`, `expert_guidance_form_clean.html` | Earlier versions of the "Get Expert Guidance" form |
| `bp_calc.html`, `bpc.html`, `bpc (2).html`, `borrowing-calculater copy.html`, `test.html`, `STAMP.html` | Old Borrowing Power drafts. `STAMP.html` is a Borrowing Power file despite its name |
| `*copy.html` files not listed above | Older copies of the calculators |
| `brand-page-xiaomi.html`, `CODE.py` | Not part of this project |

## Updating a calculator on Webflow

1. Open the calculator's local file (see the table above) in a browser and test it.
2. In the Webflow Designer, open the page and find the calculator's **Embed** elements.
3. Webflow limits each Embed to **50,000 characters**. The larger calculators are split across 2–3 Embeds (for example CSS + markup in one, the script in the next). Keep the same split when replacing code.
4. Paste only what goes inside `<body>`, plus the `<style>` block. Leave out `<html>`, `<head>` and `<title>`.
5. Publish, then test on the live page: calculate, send the contact form and book a test call.

## How each calculator works

All inputs are entered in Australian dollars. The math runs entirely in the browser, with nothing sent to a server until the visitor submits a form.

### Stamp Duty
- **Inputs:** state, first home buyer, property value, primary residence or investment, established/new/vacant land, plus state-specific questions (pensioner, income and children for ACT, foreign purchaser, paper/electronic for VIC, WA region).
- **Math:** each state has its own duty brackets in `calculate()`. The value is rounded up to the next $100, then first-home-buyer, pensioner and foreign-purchaser rules are applied, followed by the mortgage registration and transfer fees.
- **Output:** stamp duty, mortgage fee, transfer fee, total government fees, grant. State-specific first-home-buyer notes appear when "first home buyer = Yes".

### Borrowing Power
- A five-step wizard: household, income, rent, existing loans and debts, new loan.
- **Settings** (in `CALCULATOR_CONFIG`): 2025-26 tax brackets, LITO, Medicare levy, a 3% serviceability buffer on all rates, 80% of rental income counted, credit cards assessed at 3.8% of the limit per month, and HEM living expenses by household type and income band.
- **Math:** monthly surplus = net income + 80% rent + tax benefit of investments − HEM − existing repayments (at rate + 3%) − rent paid − credit cards − other debts. A binary search then finds the largest loan whose repayment fits that surplus (up to $20M).
- **Output:** maximum borrowing capacity and monthly repayment, or a "where your money went" breakdown when there is no capacity.

### Extra Repayment
- Loan amount, rate, term, repayment frequency (monthly, fortnightly or weekly), extra amount per payment, and the year the extra payments start.
- Simulates the loan period by period with and without the extra payment. Shows the minimum and increased repayment, time saved, interest saved, and a balance chart.

### Loan Comparison (Refinance)
- Loan amount and term, then upfront fees, monthly fees, intro rate, intro term (months) and ongoing rate for the existing and the new loan.
- Shows the intro and ongoing monthly repayment and total payments for each loan, a "Prospective Loan will save you $X" line, and a chart of cumulative savings.

### Loan Repayment (Home Loan Balance)
- Loan amount, rate, term, repayment frequency, plus a loan fee and its frequency.
- Shows the repayment per period (fee included), total interest and fees, total payments, and a yearly bar chart of balance vs. remaining payments.

### Rent to Buy
- Monthly rent (slider, $0–$10,000), interest rate and term.
- Loan = present value of the rent paid monthly over the term. Shows the loan figure and a principal vs. total repayments chart.

### Deposit Timeline
- Target price, current savings, monthly savings and savings interest rate (slider, 0–8%).
- Counts the months until savings (with monthly interest) reach 5%, 10%, 15% and 20% of the price. Milestones over 15 years are hidden. LMI is estimated by scaling fixed figures for a $650,000 property ($23,200 at 5%, $11,700 at 10%, $4,700 at 15%, $0 at 20%).

## Lead capture (Make.com)

| Form | Webhook | Sends |
| --- | --- | --- |
| **Send** (Contact us, on every calculator) | `hook.eu1.make.com/4mou2lewj…` (JSON) | `contact` (name, email, phone), `meta` (company *Forrentech*, lead source *Mortgage Counsel*, calculator name, date/time), plus `inputs` and `outputs` if the visitor has pressed Calculate |
| **Book a call** (calendar) | `hook.eu1.make.com/zcvj7iuo…` (form-encoded) | name, email, phone, date, time, `start_date`/`end_date` (`YYYYMMDDTHHMMSSZ`), duration 30 min |
| **Get Expert Guidance** (`ctafooter.html`, `cta_main.html`) | `hook.eu1.make.com/4mou2lewj…` | contact details, the "goals" message (250 characters max), source *Contact us Button* |

- Phone fields use a fixed `+61 4` prefix and format the remaining 8 digits as `xx xxx xxx`.
- The calendar offers 30-minute slots from 9:00 AM to 9:00 PM (no 12:00–1:00 PM slots), weekdays only, with past times disabled.
- A site-wide booking modal (`bkd…` IDs, opened by the floating calendar button) lives in Webflow's site or footer custom code and is **not in this folder**.

## Known issues and next steps

| Issue | Detail |
| --- | --- |
| Local files out of date | Extra Repayment and Loan Comparison on Webflow are newer than any local file. Export them from Webflow and keep one file per calculator. |
| No version control | `.git` contains only an empty `info` folder, so git does not recognise it. Run `git init`, keep only the live files (renamed clearly, e.g. `stamp-duty.html`) and move drafts to an `archive/` folder. |
| Webhook keys are public | The `x-make-apiKey` header values are visible in the page source, so anyone can post to the webhooks. Add validation or rate limiting in Make. |
| Booking time zone | Start and end times are the visitor's **local** clock time but are labelled UTC (`Z`). Unless the Make scenario corrects for this, a 9:00 AM booking in Sydney can appear in the calendar as 7:00 PM. |
| Booked slots aren't checked | Taken slots are only remembered in the open page. Two visitors can book the same time. |
| Errors can look like success | `fetch` only fails on network errors. If Make returns an error, the visitor still sees "Form submitted successfully!". Check `response.ok`. |
| Stamp Duty "New Home Grant" row | Always shows $0.00 (the value is never set). |
| WA vacant-land note | The note says duty applies above **$350,000**, but the code (correctly) uses $450,000. |
| Short loan terms on Extra Repayment | For terms under ~6 years, x-axis labels repeat (e.g. `0 0 0 1 1 1 1`). |
| Rates and thresholds | Tax brackets, HEM, stamp duty and grant rules reflect 2025-26 and change every year. Review them each July. |

## Brand

| Token | Hex |
| --- | --- |
| Primary (buttons, highlights) | `#ff6464` |
| Dark text / result headers | `#111827`, `#000000` |
| Borders and panels | `#e0e0e0`, `#efefef` |
| Success / error toasts | `#22c55e` / `#ef4444` |
