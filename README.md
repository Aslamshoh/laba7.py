# laba7.py
'''
Задание состоит из двух частей. 
1 часть – написать программу в соответствии со своим вариантом задания. Написать 2 варианта формирования (алгоритмический и с помощью функций Питона), сравнив по времени их выполнение.
2 часть – усложнить написанную программу, введя по своему усмотрению в условие минимум одно ограничение на характеристики объектов (которое будет сокращать количество переборов)  и целевую функцию для нахождения оптимального  решения.
Вариант 30. Вводятся К цифр. Составьте все возможные целые числа из этих цифр. Каждая цифра используется только 1 раз.
Требуется для своего варианта второй части л.р. №6 (усложненной программы) разработать реализацию с использованием графического интерфейса. Допускается использовать любую графическую библиотеку питона.  
Рекомендуется использовать внутреннюю библиотеку питона  tkinter.
В программе должны быть реализованы минимум одно окно ввода, одно окно вывода (со скролингом), одно текстовое поле, одна кнопка.
'''

import itertools
import time
from tkinter import Tk, Entry, Button, Label, Text, Scrollbar, END

def generate_integers_algorithmic(digits, S):
    digits_str = ''.join(digits)
    permutations = list(itertools.permutations(digits_str))
    valid_permutations = [permutation for permutation in permutations if sum(int(digit) for digit in permutation) >= S]
    integers = [int(''.join(permutation)) for permutation in valid_permutations]
    return integers

def generate_integers_pythonic(digits, S):
    digits_str = ''.join(digits)
    permutations = itertools.permutations(digits_str)
    valid_permutations = [permutation for permutation in permutations if sum(int(digit) for digit in permutation) >= S]
    integers = [int(''.join(permutation)) for permutation in valid_permutations]
    return integers

def calculate_and_display():
    K = int(entry_K.get())
    digits = [entry_digits[i].get() for i in range(K)]
    S = int(entry_S.get())

    # Алгоритмический подход
    start_time_algorithmic = time.time()
    integers_algorithmic = generate_integers_algorithmic(digits, S)
    end_time_algorithmic = time.time()

    # Подход с использованием функций Python
    start_time_pythonic = time.time()
    integers_pythonic = generate_integers_pythonic(digits, S)
    end_time_pythonic = time.time()

    result_text = f"Все возможные целые числа (алгоритмический подход):\n{integers_algorithmic}\nВремя выполнения алгоритмического подхода: {end_time_algorithmic - start_time_algorithmic:.6f} секунд\n\n"
    result_text += f"Все возможные целые числа (подход с использованием функций Python):\n{integers_pythonic}\nВремя выполнения подхода с использованием функций Python: {end_time_pythonic - start_time_pythonic:.6f} секунд\n\n"
    result_text += f"Максимальное целое число: {max(integers_algorithmic)}"

    output_text.config(state='normal')
    output_text.delete('1.0', END)
    output_text.insert(END, result_text)
    output_text.config(state='disabled')

# Создание главного окна
root = Tk()
root.title("Генерация чисел")

# Поля ввода
Label(root, text="Введите количество цифр:").grid(row=0, column=0, padx=10, pady=5, sticky='w')
entry_K = Entry(root)
entry_K.grid(row=0, column=1, padx=10, pady=5, sticky='w')

Label(root, text="Введите цифры:").grid(row=1, column=0, padx=10, pady=5, sticky='w')
entry_digits = []
for i in range(9):  # Максимум 9 цифр
    entry_digits.append(Entry(root, width=3))
    entry_digits[i].grid(row=1, column=i+1, padx=5, pady=5)

Label(root, text="Введите минимальную сумму цифр:").grid(row=2, column=0, padx=10, pady=5, sticky='w')
entry_S = Entry(root)
entry_S.grid(row=2, column=1, padx=10, pady=5, sticky='w')

# Кнопка вычисления
Button(root, text="Вычислить", command=calculate_and_display).grid(row=3, column=0, columnspan=10, pady=10)

# Поле вывода результатов с прокруткой
output_frame = Label(root)
output_frame.grid(row=4, column=0, columnspan=10, padx=10, pady=10)
scrollbar = Scrollbar(output_frame)
scrollbar.pack(side='right', fill='y')

output_text = Text(output_frame, height=15, width=80, wrap='word', yscrollcommand=scrollbar.set)
output_text.pack(side='left', fill='both', expand=True)
scrollbar.config(command=output_text.yview)

root.mainloop()
