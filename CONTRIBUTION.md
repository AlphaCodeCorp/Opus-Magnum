# Questions relatives aux issues

L'outil de suivi des problèmes n'est pas un forum de soutien. À moins que vous ne puissiez fournir des informations techniques précises concernant un problème, vous ne devez pas y poster. Si vous avez besoin d'aide, lisez d'abord la FAQ et visitez ensuite notre serveur Discord. Si vous postez des questions d'assistance, des messages génériques aux développeurs ou des rapports vagues sans détails techniques, ils seront fermés et verrouillés.

Si vous pensez avoir un rapport de problème valide, veuillez poster un texte ou une capture d'écran du log et de la version de l'application ainsi que vos informations sur le matériel et les logiciels, le cas échéant.

## Contribuer

AlphaCode est une toute nouvelle association créée par des étudiants pour plonger dans l'open source, nous avons une grande opportunité de garder tout propre et bien organisé dès le départ. C'est pourquoi le style de codage est très important lors de la prise d'engagements. Nous tournons sur flake8 sur notre dépôt Python pour vérifier le code. Veuillez l'utiliser pour formater votre code lorsque vous contribuez. Cependant, il ne couvre pas toutes les règles ci-dessous.

## Utiliser Flake8

Pour commencer à utiliser Flake8, ouvrez un shell et lancez

```sh
flake8 path/to/code/to/check.py
# or
flake8 path/to/code
```

### VS-Code

settings.json

```Json
{
    "python.linting.enabled" : vrai,
    "python.linting.flake8Enabled" : vrai
}
```

## Règles générales

- Une grande partie du code a été tirée d'autres projets antérieurs à AlphaCode. En général, lorsque vous éditez le code d'autres personnes, suivez le style du module dans lequel vous êtes (ou mieux encore, corrigez le style s'il diffère radicalement de notre guide)
- La largeur de ligne est généralement de 100 caractères.
- N'introduisez jamais de nouvelles dépendances externes dans le code.
- N'utilisez pas de plate-forme spécifique dans le code, si possible.

## Règles de nommage

- Fonctions : mixedCase
- Variables : lower_case_underscored. Préfixe avec g_ si global.
- Classes : PascalCase
- Fichiers et répertoires : lower_case_underscored

## Indentation/espace blanc

Suivez le style d'indentation/espace blanc indiqué ci-dessous. N'utilisez pas d'espaces, mais plutôt des tabulations.

## Commentaires

- Pour les commentaires réguliers, utilisez des commentaires de style multi-lignes ("""), même pour ceux d'une seule ligne.
- Pour les commentaires de documents, utilisez la convention Numpydoc.

```py
from discord.ext import commands
from discord import  Embed
import datetime
from reverse.core._service import SqliteService

class Mood(commands.Cog):

    REACTION = [
        "😩",
        "🙁",
        "😶",
        "😐",
        "🙂",
        "😁"
    ]

    def __init__(self, bot):
        self.bot = bot
        self.db = SqliteService()
        
    @commands.command()
    async def askme(self, ctx):
        """Send a embed to the user asking him how he is

        Parameters
        ----------
        ctx : Context
        """
        embed = Embed(title="How are you ?", color=0xe80005, timestamp=datetime.datetime.today())
        embed.add_field(name="Terrible", value=":weary:", inline=True)
        embed.add_field(name="Sad", value=":slight_frown:", inline=True)
        embed.add_field(name="Don't know", value=":no_mouth:", inline=True)
        embed.add_field(name="Ok-ish", value=":neutral_face:", inline=True)
        embed.add_field(name="Good", value=":slight_smile:", inline=True)
        embed.add_field(name="Fine", value=":grin:", inline=True)
        message = await ctx.send(embed=embed)
        
        for item in Mood.REACTION:
            await message.add_reaction(item)

    @commands.Cog.listener()
    async def on_reaction_add(self, reaction, user):
        """Event On Reaction Add

        Parameters
        ----------
        reaction : Reaction
        user : User
        """
        if(user.id != 501719851740561408):
            message = reaction.message
            await message.channel.send("Merci {} de ta réponse. ({})".format(user, reaction))
            await message.delete()
        
def setup(bot):
    bot.add_cog(Mood(bot))
```
