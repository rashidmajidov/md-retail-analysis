# MD Retail Analysis

Bu layihə İstanbulda fəaliyyət göstərən 10 ticarət mərkəzinin müştəri alış-veriş datası üzərində satış performansının analiz edilməsini əhatə edir. Məqsəd kateqoriya, ödəniş üsulu, mərkəz və cins üzrə gəlir (revenue) göstəricilərini araşdıraraq əsas KPI-ları çıxarmaqdır.

## Dataset

- **Fayl:** `customer_shopping_data.csv`
- **Həcm:** ~99,457 sətir, 10 ticarət mərkəzi, 8 məhsul kateqoriyası
- **Tarix aralığı:** 2021-01-01 – 2023-03 (2023-cü il məlumatları ilin yalnız bir hissəsini əhatə edir, ona görə illik müqayisələrdə bu nəzərə alınmalıdır)
- **Sütunlar:** `invoice_no`, `customer_id`, `gender`, `age`, `category`, `quantity`, `price`, `payment_method`, `invoice_date`, `shopping_mall`

## Layihənin Strukturu

```
md-retail-analysis/
│
├── data/
│   ├── raw/
│   │   └── customer_shopping_data.csv             # Xam dataset
│   └── processed/
│       └── customer_shopping_data_cleaned.csv     # Təmizlənmiş dataset
│
├── notebooks/
│   ├── 01_data_cleaning.ipynb                     # Data təmizləmə prosesi
│   └── 02_eda.ipynb                                # KPI hesablamaları və analiz
│
├── reports/
│   └── figure/                                     # Analiz zamanı yaradılan qrafiklər
│       ├── heatmap.png
│       ├── yearly_revenue_category_logscale.png
│       ├── paymentmethods_pie.png
│       ├── revenue_by_4labels.png
│       ├── avg_revenue_bycatg.png
│       └── gender_categorical.png
│
├── main.py
├── pyproject.toml
├── uv.lock
└── README.md
```

## İstifadə Olunan Texnologiyalar

- Python 3.13+ (mühit `uv` ilə idarə olunur)
- pandas, numpy — data emalı
- matplotlib, seaborn, plotly — vizuallaşdırma

## 1. Data Təmizləmə (`01_data_cleaning.ipynb`)

- Dataset strukturu (`shape`, `info`, `describe`), boş və təkrarlanan dəyərlər yoxlanılıb (boş/duplicate sətir aşkarlanmayıb)
- `invoice_date` sütunu `datetime` formatına çevrilib, bundan `invoice_month` və `invoice_year` sütunları çıxarılıb
- `revenue` sütunu (`price × quantity`) hesablanaraq əlavə edilib
- Nəticə `data/processed/customer_shopping_data_cleaned.csv` olaraq yadda saxlanılıb

## 2. Analiz və KPI-lar (`02_eda.ipynb`)

Hesablanan əsas göstəricilər hər kəsit üzrə: **Total Spending**, **Average Spending**, **Transaction Count**.

- Kateqoriya, ödəniş üsulu, mərkəz və cins üzrə ümumi/ortalama gəlir və əməliyyat sayı
- Mərkəz vs kateqoriya üzrə gəlir heatmap-i (`heatmap.png`)
- İllər üzrə kateqoriya bazasında gəlir trendi (`yearly_revenue_category_logscale.png`)
- Cins və kateqoriya üzrə xərcləmə müqayisəsi (`gender_categorical.png`)
- Mərkəz üzrə ay-ay (Month-over-Month) gəlir artım/azalma faizi
- Kateqoriya üzrə hansı ödəniş üsulunun daha çox istifadə olunduğu (`paymentmethods_pie.png`)

## Əsas Göstəricilər (Nümunə Nəticələr)

| Kateqoriya | Ümumi Gəlir | Ortalama Çek | Əməliyyat Sayı |
|---|---|---|---|
| Clothing | ~114M | ~3,306 | 34,487 |
| Shoes | ~66.6M | ~6,633 | 10,034 |
| Technology | ~57.9M | ~11,582 | 4,996 |
| Cosmetics | ~6.8M | ~450 | 15,097 |
| Toys | ~4.0M | ~395 | 10,087 |
| Food & Beverage | ~0.85M | ~57 | 14,776 |
| Books | ~0.83M | ~168 | 4,981 |
| Souvenir | ~0.64M | ~127 | 4,999 |

**Ödəniş üsulu üzrə:** Cash (~113M), Credit Card (~88M), Debit Card (~50.6M) — üç üsul da təxminən yaxın orta çek dəyərinə (~2,520) malikdir və hər kateqoriyada nağd ödəniş (Cash) ən çox istifadə olunan üsuldur (~44-46% əməliyyat payı ilə).

## Biznes İnsightları

- Ən yüksək ümumi gəliri Clothing kateqoriyası yaradır, lakin ortalama çek üzrə Technology qat-qat öndədir — bu, az sayda, lakin yüksək dəyərli əməliyyatlar deməkdir.
- Ödəniş üsulları arasında ortalama çek dəyəri ciddi fərqlənmir; bu, ödəniş metodunun alış-veriş məbləğinə deyil, daha çox müştəri vərdişinə bağlı olduğunu göstərir.
- Bütün kateqoriyalarda nağd ödəniş digər üsullardan bir qədər öndədir, amma fərq böyük deyil (44-46% aralığında) — üç ödəniş üsulu da təxminən bərabər paya malikdir.
- Cins üzrə xərcləmədə qadın müştərilər bütün kateqoriyalarda kişi müştərilərdən daha yüksək ümumi gəlir gətirir; bu fərq müştəri bazasının tərkibindən (say nisbətindən) irəli gələ bilər və əlavə araşdırma (məs. müştəri sayına normallaşdırma) tələb edir.
- 2023-cü il rəqəmləri digər illərdən nəzərəçarpacaq dərəcədə aşağıdır, lakin bu, satışların azalması demək deyil — dataset 2023-cü il üçün yalnız qismən (yanvar-mart) məlumat ehtiva edir.

## Necə İşə Salmaq

Layihə `uv` ilə idarə olunur:

```bash
uv sync
uv run jupyter notebook notebooks/01_data_cleaning.ipynb
```
