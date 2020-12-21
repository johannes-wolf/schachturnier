---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
---

![Schachturnier Banner](img/banner.png)

# Regeln
* Spiele werden auf [lichess.org](https://lichess.org) ausgetragen
* Jeder Spieler/jede Spielerin spielt ein „Best-Of-Three“ Spiel gegen jeden Teilnehmer/jede Teilnehmerin
* Spieldauer is 10+0 (10 Minuten, kein increment)

# Spielplan

{% for player in site.data.players %}
## [{{ player.name }}](https://lichess.org/@/{{ player.lichess }})

<table class="game-result">
    {% for opponent in site.data.players %}
    {% unless opponent.name == player.name %}
    <tr>
        {% assign round = false %}

        <!-- Find matching game data -->
        {% for g in site.data.games %}
            {% if g.a == player.name and g.b == opponent.name %}
                {% assign is-player = "a" %}
                {% assign round = g %}
                {% break%}
            {% else if g.b == player.name and g.a == opponent.name %}
                {% assign is-player = "b" %}
                {% assign round = g %}
                {% break%}
            {% endif %}
        {% endfor %}
    
        <!-- Print match table -->
        <td>{{ opponent.name }}</td>
        <td>
            {% assign score = 0 %}
            {% for game in round.games %}
                <!-- Var 'is-player' is either 'a' or 'b' -->
                {% if game.win == is-player %}
                    {% assign score = score | plus: 1 %}
                {% endif %}
            {% endfor %}
            
            {% if round %}
            {{ score }}
            {% else %}
            -
            {% endif %}
        </td>
        <td>
        {% if round %}
            {% for game in round.games %}
                <div><a href="https://lichess.org/{{ game.id }}">Spiel {{ forloop.index }}</a></div>
            {% endfor %}
        {% else %}
            Ausstehend
        {% endif %}
        </td>
    </tr>
    {% endunless %}
    {% endfor %}
</table>
{% endfor %}
