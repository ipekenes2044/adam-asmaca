# adam-asmaca
python ile adam asmaca oyunu  yaptım oyunu yaparken random ve tkinter kütüphanelerini kullandım


import tkinter as tk
import random

# Kelime Listesi
kelimeler = ["python", "programlama", "bilgisayar", "oyun", "geliştirici", "yazılım", "algoritma", "değişken",
             "fonksiyon", "modül"]
kelime = random.choice(kelimeler)
kelime_tahmin = ["-" for _ in kelime]

# Başlangıç Değerleri
can_sayisi = 6
tahmin_edilen_harfler = []

# Tkinter Penceresi
root = tk.Tk()
root.title("Adam Asmaca Oyunu")
root.geometry("600x400")
root.resizable(False, False)

# Adam Asmaca Çizimi İçin Canvas
canvas = tk.Canvas(root, width=200, height=250, bg="white")
canvas.pack(pady=20, side=tk.LEFT)

# Kelime ve Mesaj Etiketleri
info_frame = tk.Frame(root)
info_frame.pack(pady=20, padx=20, side=tk.RIGHT, fill=tk.BOTH, expand=True)

kelime_label = tk.Label(info_frame, text=" ".join(kelime_tahmin), font=("Helvetica", 24))
kelime_label.pack(pady=20)

mesaj_label = tk.Label(info_frame, text="", font=("Helvetica", 16), fg="red")
mesaj_label.pack(pady=10)

# Harf Giriş ve Buton
giris_frame = tk.Frame(info_frame)
giris_frame.pack(pady=10)

harf_giris = tk.Entry(giris_frame, font=("Helvetica", 18), width=5)
harf_giris.pack(side=tk.LEFT, padx=5)

tahmin_butonu = tk.Button(giris_frame, text="Tahmin Et", command=lambda: harf_tahmin_et())
tahmin_butonu.pack(side=tk.LEFT, padx=5)

# Kalan Can Etiketi
can_label = tk.Label(info_frame, text=f"Kalan Can: {can_sayisi}", font=("Helvetica", 14))
can_label.pack(pady=10)

# Yanlış Tahminler Etiketi
yanlis_label = tk.Label(info_frame, text="Yanlış Tahminler: ", font=("Helvetica", 14))
yanlis_label.pack(pady=10)


# Adam Asmaca Çizim Fonksiyonu
def adam_ciz(adim):
    if adim == 1:
        # Çerçeveyi çiz
        canvas.create_line(50, 230, 150, 230)  # Zemin
    elif adim == 2:
        canvas.create_line(100, 230, 100, 20)  # Direk
    elif adim == 3:
        canvas.create_line(100, 20, 180, 20)  # Üst direk
    elif adim == 4:
        canvas.create_line(180, 20, 180, 50)  # Halat
    elif adim == 5:
        canvas.create_oval(160, 50, 200, 90)  # Kafa
    elif adim == 6:
        canvas.create_line(180, 90, 180, 150)  # Gövde
    elif adim == 7:
        canvas.create_line(180, 100, 160, 130)  # Sol Kol
    elif adim == 8:
        canvas.create_line(180, 100, 200, 130)  # Sağ Kol
    elif adim == 9:
        canvas.create_line(180, 150, 160, 180)  # Sol Bacak
    elif adim == 10:
        canvas.create_line(180, 150, 200, 180)  # Sağ Bacak


# Kelimeyi Güncelleme Fonksiyonu
def kelimeyi_guncelle():
    kelime_label.config(text=" ".join(kelime_tahmin))


# Harf Tahmin Etme Fonksiyonu
def harf_tahmin_et():
    global can_sayisi
    tahmin = harf_giris.get().lower()
    harf_giris.delete(0, tk.END)

    if not tahmin.isalpha() or len(tahmin) != 1:
        mesaj_label.config(text="Lütfen tek bir harf girin.")
        return

    if tahmin in tahmin_edilen_harfler:
        mesaj_label.config(text=f"{tahmin} harfini zaten tahmin ettiniz.")
        return

    if tahmin in kelime:
        mesaj_label.config(text=f"Tebrikler! '{tahmin}' harfi doğru.")
        for i, harf in enumerate(kelime):
            if harf == tahmin:
                kelime_tahmin[i] = tahmin
    else:
        mesaj_label.config(text=f"Maalesef! '{tahmin}' harfi yanlış.")
        can_sayisi -= 1
        adam_ciz(6 - can_sayisi)  # Her yanlış tahminde bir adım çiz

    tahmin_edilen_harfler.append(tahmin)
    kelimeyi_guncelle()
    can_label.config(text=f"Kalan Can: {can_sayisi}")
    yanlis_label.config(text=f"Yanlış Tahminler: {', '.join([h for h in tahmin_edilen_harfler if h not in kelime])}")

    if "-" not in kelime_tahmin:
        mesaj_label.config(text="Tebrikler, kelimeyi buldunuz!", fg="green")
        tahmin_butonu.config(state=tk.DISABLED)
    elif can_sayisi == 0:
        mesaj_label.config(text=f"Üzgünüm, kaybettiniz! Kelime: {kelime}", fg="red")
        kelime_label.config(text=" ".join(kelime))
        tahmin_butonu.config(state=tk.DISABLED)


# Başlangıç Çizimi
adam_ciz(1)  # Zemin çiz
adam_ciz(2)  # Direk çiz

# Oyunu Başlat
kelimeyi_guncelle()
root.mainloop()
