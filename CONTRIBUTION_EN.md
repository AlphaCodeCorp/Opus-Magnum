# Reporting Issues

The issue tracker is not a support forum. Unless you can provide precise technical information regarding an issue, you should not post in it. If you need support, first read the FAQ and then visit our Discord server. If you post support questions, generic messages to the developers or vague reports without technical details, they will be closed and locked.

If you believe you have a valid issue report, please post text or screenshot from the log and build version as well as your hardware and software information if applicable.

## Contributing

AlphaCode is a brand new association created by students to dive into open source, we have a great opportunity to keep everything clean and well organized early on. As such, coding style is very important when making commits. We run on flake8 on our Python repository to check the code. Please use it to format your code when contributing. However it doesn't cover all the rules below.

## Using Flake8 format

To start using Flake8, open an interactive shell and run

```sh
flake8 path/to/code/to/check.py
# or
flake8 path/to/code
```

### VS-Code

settings.json

```json
{
    "python.linting.enabled": true,
    "python.linting.flake8Enabled": true
}
```

## General Rules

- A lot of code was taken from other projects that predate AlphaCode. In general, when editing other people's code, follow the style of the module you're in (or better yet, fix the style if it drastically differs from our guide)
- Line width is typically 100 characters.
- Don't ever introduce new external dependencies into the code.
- Don't use any platform specific in the code, if possible.

## Naming Rules

- Functions: mixedCase
- Variables: lower_case_underscored. Prefix with g_ if global.
- Classes: PascalCase
- Files and Directories: lower_case_underscored

## Indentation/Whitespace Style

Follow the indentation/whitespace style shown below. Do not use spaces, use tabs instead.

## Comments

- For regular comments, use multi-lines style (""") comments, even for one-line ones.
- For doc-comments, use Numpydoc convention.

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