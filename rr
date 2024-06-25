import fitz  # PyMuPDF
import re

def find_first_percentage_after_keyword(pdf_path, keyword):
    # Abre o PDF
    document = fitz.open(pdf_path)
    
    # Extrai o texto de todas as páginas
    text = ""
    for page_num in range(len(document)):
        page = document.load_page(page_num)
        text += page.get_text()
    
    # Encontra todas as ocorrências da palavra-chave
    keyword_positions = [m.start() for m in re.finditer(keyword, text, re.IGNORECASE)]
    
    for pos in keyword_positions:
        # Encontra o parágrafo onde a palavra-chave foi encontrada
        start = text.rfind('\n', 0, pos) + 1
        end = text.find('\n', pos)
        if end == -1:
            end = len(text)
        
        paragraph = text[start:end].strip()
        print(f"Parágrafo encontrado para a palavra-chave '{keyword}':\n{paragraph}\n")
        
        # Procura por valores em % e valores decimais no parágrafo
        percentage_match = re.search(r'\b\d{1,2}(?:,\d{1,2})?%|\b0?,\d{1,2}(?:,\d{1,2})?', paragraph)
        
        if percentage_match:
            value = percentage_match.group()
            return value
    
    return None

def find_percentages_for_keywords(pdf_path, keywords):
    results = {}
    for keyword in keywords:
        result = find_first_percentage_after_keyword(pdf_path, keyword)
        if result:
            results[keyword] = result
        else:
            results[keyword] = "Nenhum valor percentual encontrado."
    return results

# Caminho para o arquivo PDF
pdf_path = 'p.pdf'

# Palavras-chave a serem procuradas
keywords = ["Estruturação"]

# Chama a função e obtém os resultados
results = find_percentages_for_keywords(pdf_path, keywords)

# Imprime os resultados
for keyword, value in results.items():
    print(f"Resultado para '{keyword}': {value}")
