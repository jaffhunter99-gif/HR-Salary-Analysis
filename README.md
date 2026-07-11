# HR Salary Analysis

SQL və Tableau istifadə edərək şirkət daxilində maaş bərabərsizliyinin departament, vəzifə və təcrübə üzrə analizi.

## Məqsəd

Bu layihə aşağıdakı sualları cavablandırır:

- Hansı departamentdə orta maaş ən yüksəkdir?
- Hansı vəzifədə (job title) maaş fərqi (min-max gap) ən böyükdür və niyə?
- Təcrübə (iş stажı) ilə maaş arasında əlaqə varmı?
- Şirkətin ümumi maaş büdcəsi departamentlər arasında necə bölünür?

## Verilənlər bazası

Oracle-ın standart **HR (Human Resources)** nümunə sxemi istifadə olunub. Sxem aşağıdakı cədvəllərdən ibarətdir:

- `EMPLOYEES` – 107 işçi (ad, maaş, işə giriş tarixi, vəzifə, departament)
- `DEPARTMENTS` – 27 departament
- `JOBS` – 19 vəzifə (hər biri üçün min/max maaş aralığı)
- `JOB_HISTORY` – işçilərin vəzifə dəyişikliyi tarixçəsi
- `LOCATIONS`, `COUNTRIES`, `REGIONS` – coğrafi məlumat

## İstifadə olunan alətlər

- **Oracle SQL** – analitik sorğular (JOIN, GROUP BY, aggregate funksiyalar)
- **Tableau** – dashboard və vizuallaşdırma

## Metodologiya

1. `EMPLOYEES`, `DEPARTMENTS` və `JOBS` cədvəlləri SQL-də birləşdirilib, departament və vəzifə üzrə maaş göstəriciləri (orta, minimum, maksimum) hesablanıb
2. Hər vəzifə daxilində maaş fərqini (`MAX(SALARY) - MIN(SALARY)`) tapmaq üçün ayrıca sorğu yazılıb
3. `HIRE_DATE` əsasında işçilərin təcrübə ili hesablanıb və maaşla müqayisə olunub
4. Departament üzrə ümumi maaş xərci (`SUM(SALARY)`) hesablanıb ki, hansı departamentin büdcədə ən böyük paya sahib olduğu görünsün
5. Nəticələr Tableau-da 4 qrafikli dashboard-da vizuallaşdırılıb

SQL sorğularının hamısı [`hr_salary_analysis.sql`](./hr_salary_analysis.sql) faylındadır.

## Dashboard

Dashboard-u interaktiv görmək üçün [`dashboard.twbx`](./dashboard.twbx) faylını endirib Tableau Desktop-da açın (Tableau Public pulsuz mövcuddur).

![HR Salary Dashboard](./dashboard.png)

Dashboard 4 hissədən ibarətdir:
- **Avg Salary by Department** – departament üzrə orta maaş müqayisəsi
- **Salary Gap by Job Title** – hər vəzifə daxilində maaş fərqi
- **Years Experience vs Salary** – təcrübə ilə maaş arasındakı əlaqə (departament üzrə rəngli)
- **Salary Distribution by Department** (Treemap) – departamentlərin ümumi maaş büdcəsindəki payı

## Əsas nəticələr

**1. Executive departamenti ən yüksək orta maaşa malikdir (~19,333)**, amma ən az sayda işçini əhatə edir. Şirkətin ən böyük işçi qrupu **Shipping** departamentindədir (45 nəfər), lakin bu departamentdə orta maaş ən aşağıdır (~3,475).

**2. Sales Representative vəzifəsində maaş fərqi ən böyükdür.** Eyni vəzifədə işləyən işçilər arasında əhəmiyyətli maaş fərqi olması, bu vəzifədə komissiya və ya təcrübə əsaslı fərqləndirmənin mövcud olduğunu göstərir.

**3. Təcrübə ilə maaş arasında gözlənilən qədər güclü əlaqə yoxdur.** Bəzi uzun müddət işləyən işçilər nisbətən aşağı maaş alır, bu da onu göstərir ki, **maaşı müəyyən edən əsas amil departament/vəzifədir, təcrübə ili deyil**.

**4. Maaş büdcəsi bərabər bölünməyib** – Executive və Finance departamentləri ümumi maaş xərcinin böyük hissəsini təşkil edir, halbuki bir çox kiçik departament büdcənin cüzi faizini alır.

## Ümumi nəticə

Şirkətdə maaş bərabərsizliyi əsasən **departament və vəzifə səviyyəsindən** qaynaqlanır, işçinin təcrübə ilindən deyil. Bu, HR strategiyası qurarkən diqqət yetirilməli əsas məqamdır.
