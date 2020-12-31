---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
---

![Schachturnier Banner](img/banner.png)

# Regeln
* Spiele werden auf [lichess.org](https://lichess.org) ausgetragen.
* Jeder Spieler/jede Spielerin spielt ein „Best-Of-Three“ Satz gegen jeden Teilnehmer/jede Teilnehmerin.
* Die Spieldauer beträgt 10+0 (10 Minuten, 0 Sekunden inkrement), abwechselnd Schwarz & Weiß.
* Nach dem Spiel: Spielergebnis und Link zum Spiel (Replay) im Telegramkanal posten.
* Ein Sieg zählt einen Punkt; der Spieler oder die Spielerin mit den meisten Punkten gewinnt das Turnier.
* Endet das Turnier unentschieden, spielen die Gewinner jeweils ein „Best-Of-Three“ Satz Finale.

# Spielplan

{% for player in site.data.players %}
## [{{ player.name }}](https://lichess.org/@/{{ player.lichess }}) {% if player.invalid %} - ausgeschieden {% endif %}

<table class="game-result">
    {% assign total_score = 0 %}

    <tr>
        <th>Gegner</th>
        <th>Punkte</th>
        <th>Spiele</th>
    </tr>

    {% for opponent in site.data.players %}
    {% unless opponent.name == player.name or opponent.invalid %}
    <tr>
        {% assign round = false %}

        <!-- Find matching game data -->
        {% for g in site.data.games %}
            {% if g.a == player.name and g.b == opponent.name %}
                {% assign is-player = "a" %}
                {% assign round = g %}
                {% break %}
            {% endif %}
            {% if g.b == player.name and g.a == opponent.name %}
                {% assign is-player = "b" %}
                {% assign round = g %}
                {% break %}
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
            {% assign total_score = total_score | plus: score %}
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
    
    <tr>
        <th>Summe</th>
        <th>{{ total_score }}</th>
    </tr>
</table>
{% endfor %}
