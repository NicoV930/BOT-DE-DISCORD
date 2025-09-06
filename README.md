# BOT-DE-DISCORD
# si vez un falta de ortgrafia y mala escritura me los dice para hacelo mas bonito y limpio el codigo. este es mi discord REY_monkey43, para hblar sobre el proyecto :)

import datetime
import discord
from discord.ext import commands
import requests
from src import secret
import yt_dlp
"""""
lista para hecer en el codigo y mejora el BOTS.
1. agregar esto siguente comnado:
1. comando '$Sear_web' para ser busqueda en Google y que envier por chat el primer resultado en url.
2. comando '$search_music' para ser busqueda en en spotyfa con el nombre la musicar o auto, despues enviar el resultado con la opcion de reprodur la version corta de la musicar.
3. comando '$spam' enviar texto que le poner, despue el bot te pide la cantida y tiempo (en Segundos), '$Spam_On' para terminar el spam.
4. comando '$YT_Donw' introducir la URL y despues el bot da opcione para auio y video.
"""""
# Configuration del BOT, Comandos y la connexion al server de Discord (Servidor de REY_monkey36)
intents = discord.Intents.default()
intents.message_content = True

bot = commands.Bot(command_prefix='$', intents=intents)

# comando de Pokemon
@bot.command()
async def poke(ctx, arg):
    try:
        pokemon = arg.split(" ", 1)[0].lower()
        results = requests.get("https://pokeapi.co/api/v2/pokemon/" + pokemon)

        if results.status_code != 200:
            await ctx.send("No se encontra el Pokémos")
        else:
            imagen_url = results.json()['sprites']['front_default']
            print(imagen_url)
            await ctx.send(imagen_url)
    # devolver Error en chat
    except Exception as e:
        print("ERROR!", e)


@poke.error
async def error_type(ctx, error):
    if isinstance(error, commands.errors.MissingRequiredArgument):
        await ctx.send("pasame un pokemon :)")


# comando de calculadora
@bot.command()
async def mate(ctx, *args):
    try:
        entrada = " ".join(args)
        permitted = "0123456789+-*/(). "
        if all(c in permitted for c in entrada):
            resultado = eval(entrada)
            await ctx.send(f"Resultado de la operación: {entrada} = {resultado}")
            if resultado == 13:
                await ctx.send("Mientra Más me la mame, Más me crece. AlE GAY!!")

        # devolver Error en chat
        else:
            await ctx.send("Expression no válida. Solo se permiten números y + - * / ( ) .")

    except Exception as e:
        print(f"Error: {e}")

#Comando para busqueda de video de youtube
@bot.command()
async  def YT_search(ctx, *, query: str):
    """Busca un video en YouTube y devuelve el primer resultado."""
    # Crea las opciones de búsqueda para yt-dlp
    ydl_opts = {
        'format': 'bestaudio/best',
        'quiet': True,
        'extract_flat': True,
        'force_generic_extractor': True,
    }
    try:
        # Crea un objeto yt-dlp
        with yt_dlp.YoutubeDL(ydl_opts) as ydl:
            # Busca el primer video que coincida con la consulta
            info = ydl.extract_info(f"ytsearch1:{query}", download=False)

            # Verifica si se encontró un video
            if 'entries' in info and info['entries']:
                video_url = info['entries'][0]['url']
                await ctx.send(f'Se encontró un video: {video_url}')
            else:
                await ctx.send('No se encontraron videos para tu búsqueda. Intenta con otra palabra clave.')

    except Exception as e:
        await ctx.send(f'Ocurrió un error al buscar: {e}')

#comando Spam
@bot.command()
async  def spam(ctx, *args, str, num1: int, num2: int):
    entrada = ' '.join(args)
    a, b = num1, num2
    # Números dentro de paréntesis

    if num1 and num2 == 13:
      await ctx.send("gay")
      await ctx.send(f"hola")
    else: print(entrada)


#Comando para ver informacion en eñ chat
@bot.command()
async def info(ctx):
    Dato = discord.Embed(title=f"{ctx.guild.name}", description="lorem impsum asdasd", timestamp=datetime.datetime.utcnow(), color = discord.Color.blue())
    Dato.add_field(name="Fecha de cracion del Servidor | ", value=f"{ctx.guild.created_at}")
    Dato.add_field(name="Dueño del Servidor | ", value=f"{ctx.guild.owner}")
    Dato.add_field(name="Mienbros del Servidor|", value=f"{ctx.guild.member_count}")
    Dato.add_field(name="ID del Servidor", value=f"{ctx.guild.id}")
    Dato.set_thumbnail(url='https://id-test-11.slatic.net/p/7fedeec911d49f817e5b55f884820a2e.jpg')
    await  ctx.send(embed=Dato)


# Imprimir en la Console para verificar la connexion del Bot al server por Terminal :)
@bot.event
async def on_ready():
    print(f"Esta listo!{bot.user}")

# Token del MR.ROBOT
bot.run(secret.Token)
