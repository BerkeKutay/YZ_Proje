# Beyin Tümörü Sınıflandırma

MRI görüntülerinden beyin tümörü tespiti ve sınıflandırması için kapsamlı bir karşılaştırmalı çalışma. 6 derin öğrenme mimarisi, 4 geleneksel makine öğrenmesi modeli ve 1 hibrit yaklaşım karşılaştırılmıştır.

**Veri Seti:** [Brain MRI Dataset – Mendeley Data](https://data.mendeley.com/datasets/zwr4ntf94j/5) &nbsp;|&nbsp; **En İyi Sonuç:** %96.94 Accuracy (SVM)

---

## Sınıflandırılan Durumlar

| Sınıf | Açıklama |
|---|---|
| **Glioma** | Beynin glial hücrelerinden kaynaklanan tümör |
| **Meningioma** | Meninkslerden kaynaklanan, genellikle iyi huylu tümör |
| **Pituitary** | Hipofiz bezinde oluşan tümör |
| **No Tumor** | Tümör tespit edilmemiş sağlıklı MRI |

---

## Sonuçlar

### Derin Öğrenme Modelleri

| Model | Accuracy (%) | F1-Score (%) |
|---|---|---|
| ResNet50 | 91.14 | 91.11 |
| VGG16 | 91.14 | 91.11 |
| EfficientNetB0 | 88.63 | 88.52 |
| VGG19 | 87.04 | 86.91 |
| Custom CNN | 78.06 | 77.22 |
| MobileNetV2 | 73.53 | 70.41 |

### Geleneksel Makine Öğrenmesi Modelleri

*EfficientNetB0 ile çıkarılan özellik vektörleri üzerinde eğitilmiştir.*

| Model | Accuracy (%) | F1-Score (%) |
|---|---|---|
| **SVM** ⭐ | **96.94** | **96.93** |
| **EfficientNetB0 + SVM (hibrit)** | **96.94** | **96.93** |
| Logistic Regression | 91.75 | 91.71 |
| KNN | 91.56 | 91.44 |
| Random Forest | 86.37 | 86.13 |

> **Temel Bulgu:** Transfer learning ile özellik çıkarımı yapıldığında geleneksel ML modelleri, uçtan uca eğitilen derin öğrenme modellerini geride bırakmaktadır. EfficientNetB0 özellik çıkarıcı olarak kullanıldığında SVM %96.94 accuracy'ye ulaşmıştır.

---

## Yöntem

```
MRI Görüntüleri
     │
     ├─ Duplicate Temizleme
     ├─ Data Augmentation
     │
     ├─► Transfer Learning ──► EfficientNetB0 / ResNet50 / VGG16 / VGG19 / MobileNetV2
     ├─► Custom CNN         ──► Sıfırdan eğitim
     │
     └─► Feature Extraction (EfficientNetB0)
              │
              └─► SVM / Logistic Regression / KNN / Random Forest
```

- Sınıf dengesizliği için **data augmentation** ve **class weights** kullanılmıştır
- Erken durdurma için **EarlyStopping**, öğrenme oranı ayarı için **ReduceLROnPlateau** kullanılmıştır
- Tüm modeller için **confusion matrix**, **ROC eğrisi** ve **overfitting analizi** yapılmıştır

---

## Kurulum

```bash
pip install -r requirements.txt
jupyter notebook brain_tumor_classification.ipynb
```
