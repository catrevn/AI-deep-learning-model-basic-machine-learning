# AI-deep-learning-model-basic-machine-learning. 
# This is the model of IGTv1 (my chatbot). You can use it to reference how the AI ​​model works.


import tkinter as tk
from tkinter import scrolledtext
from machinelearningdata import search_wikipedia
from deeplearningdata import find_cuda_response
from stopwords_vi import stopwords_vi

# Tạo cửa sổ chính
root = tk.Tk()
root.title("IGTv1")
root.geometry("600x600")
root.config(bg="white")

# Khung nhập câu hỏi
entry_label = tk.Label(root, text="Nhập câu hỏi của bạn:", bg="white")
entry_label.pack(pady=10)

entry = tk.Text(root, height=2, width=50)
entry.pack()

# Khu vực trả lời
answer_label = tk.Label(root, text="Câu trả lời:", bg="white")
answer_label.pack(pady=10)

answer_text = scrolledtext.ScrolledText(root, height=24, width=60, wrap=tk.WORD, state='disabled')
answer_text.pack(pady=10)

# Hàm lọc từ đệm
# Có thể xem clear-stopwords_vi ở trong profile của tôi để hoàn thiện phần này!

# Hàm tìm kiếm và kết hợp kết quả
def fetch_combined_answer(query):
    filtered_query = remove_stopwords(query)

    # Tìm kiếm tổng quan từ machinelearningdata
    overall_info = search_wikipedia(filtered_query)
    
    # Tìm kiếm chi tiết từ deeplearningdata
    detailed_info = find_cuda_response(filtered_query)

    # Kết hợp kết quả
    return f"{overall_info}\n\n{detailed_info}"

# Nút gửi câu hỏi
def send_question():
    user_ask = entry.get(1.0, tk.END).strip()
    entry.delete(1.0, tk.END)
    answer_text.configure(state='normal')
    answer_text.delete(1.0, tk.END)

    answer = fetch_combined_answer(user_ask)
    answer_text.insert(tk.END, answer)
    answer_text.configure(state='disabled')

# Nút gửi
send_button = tk.Button(root, text="Gửi", command=send_question, bg="black", fg="white")
send_button.pack(pady=10)

# Chạy giao diện
root.mainloop()

