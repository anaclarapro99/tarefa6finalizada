import streamlit as st
import requests
from datetime import datetime

# Configuração da página
st.set_page_config(page_title="🌤️ Dashboard Climático", layout="centered")

# Título
st.title("🌤️ Dashboard Climático com OpenWeatherMap API")

# Entrada do usuário
cidade = st.text_input("Digite o nome da cidade:", "Natal")

# Chave da API (substitua pela sua chave real)
api_key = "SUA_CHAVE_API"

# Função para obter dados climáticos
def obter_dados_climaticos(cidade, api_key):
    url = f"http://api.openweathermap.org/data/2.5/weather?q={cidade}&appid={api_key}&lang=pt_br&units=metric"
    resposta = requests.get(url)
    if resposta.status_code == 200:
        return resposta.json()
    else:
        return None

# Botão para buscar dados
if st.button("Buscar"):
    dados = obter_dados_climaticos(cidade, api_key)
    if dados:
        st.subheader(f"Clima em {cidade.title()} - {datetime.now().strftime('%d/%m/%Y %H:%M:%S')}")
        st.write(f"**Descrição:** {dados['weather'][0]['description'].capitalize()}")
        st.write(f"**Temperatura:** {dados['main']['temp']} °C")
        st.write(f"**Sensação Térmica:** {dados['main']['feels_like']} °C")
        st.write(f"**Umidade:** {dados['main']['humidity']}%")
        st.write(f"**Velocidade do Vento:** {dados['wind']['speed']} m/s")
    else:
        st.error("Cidade não encontrada ou erro na requisição.")
        
