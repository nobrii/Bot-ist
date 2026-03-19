# Bot-ist
## Chat Bot feito em python com API do telegram.

#Código
```C
from telegram import Update   #Quando alguém manda uma mensagem para o bot, o Telegram envia um Update para o seu programa.
from telegram.ext import ApplicationBuilder, MessageHandler, filters, ContextTypes
import random
import unicodedata

TOKEN = ""

def remover_acentos(texto):
    return ''.join(
        c for c in unicodedata.normalize('NFD', texto)
        if unicodedata.category(c) != 'Mn'
    )

def identificar_intencao(pergunta):

    pergunta = remover_acentos(pergunta.lower())
    palavras = pergunta.split() #vai quebrar a pergunta em palavras individuais, ficando mas fácil de identificar as intenções do usuário.

    if "bom dia" in pergunta or "bomdia" in pergunta:
        return "bom_dia"

    if "boa tarde" in pergunta or "boatarde" in pergunta:
        return "boa_tarde"

    if "boa noite" in pergunta or "boanoite" in pergunta:
        return "boa_noite"
    
    if any(p in palavras for p in ["tudo bem","tudo bem?"]):
        return "educacao"

    if any(p in palavras for p in ["oi","ola","eai","opa","salve","fala","falae","oii","oie","eae","eaii"]):
        return "cumprimento"

    if "hiv" in palavras or "aids" in palavras:
        return "hiv"

    if "sifilis" in palavras:
        return "sifilis"

    if "hpv" in palavras:
        return "hpv"

    if any(p in palavras for p in ["prevenir","prevencao","evitar","proteger","protejo"]):
        return "prevencao"

    if any(p in palavras for p in ["sintoma","sintomas"]):
        return "sintomas"

    return "desconhecido"


respostas = {

"bom_dia":[
"Bom dia! Como posso ajudar sobre IST?"
],

"educacao":[
"Estou muito bem, obrigado por perguntar! Tem alguma dúvida sobre IST que eu possa ajudar?"
],

"boa_tarde":[
"Boa tarde! Posso ajudar com dúvidas sobre saúde sexual."
],

"boa_noite":[
"Boa noite! Se tiver dúvidas sobre IST estou aqui."
],

"cumprimento":[
"Olá! Posso ajudar com prevenção ou sintomas de IST."
],

"hiv":[
"O HIV é um vírus que ataca o sistema imunológico."
],

"sifilis":[
"A sífilis é uma infecção bacteriana que pode causar feridas."
],

"hpv":[
"O HPV é um vírus transmitido por contato sexual."
],

"prevencao":[
"A principal prevenção é usar camisinha e fazer exames."
],

"sintomas":[
"Alguns sintomas são feridas, corrimento ou dor ao urinar."
],

"desconhecido":[
"Você pode perguntar sobre prevenção, sintomas ou doenças como HIV."
]

}

async def responder(update: Update, context: ContextTypes.DEFAULT_TYPE):

    pergunta = update.message.text #Pega o texto da mensagem que o usuário enviou para o bot. Utilizando a propriedade text do objeto message do Update.

    intencao = identificar_intencao(pergunta)

    resposta = random.choice(respostas[intencao])

    await update.message.reply_text(resposta)


app = ApplicationBuilder().token(TOKEN).build()

app.add_handler(MessageHandler(filters.TEXT, responder))

print("Bot rodando...")

app.run_polling()
 ```C
