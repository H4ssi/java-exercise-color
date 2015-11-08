## Bilder

Ein Bild wird beschrieben durch das `interface Image`:

```
interface Image {
	Color getColorAtPosition(double x, double y);
}
```

D.h. für ein gegebenes `Image` können Sie erfragen, welche `Color`s an welcher Position (`x`/`y`) hinterlegt sind.
Für unsere Zwecke gilt, dass die Koordinaten so funktionieren wie mathematisch üblich: `-1000/-1000` ist ein Punkt "links unten", `1000/1000` ist ein Punkt "rechts oben".

Den Typ `Color` werden Sie im Laufe der Übung herausarbeiten.

__Hinweis__: Für diese Übung, können Sie Ihre Typen "immutable" (deutsch: "unveränderlich") gestalten: Ein "immutable" Objekt kann (nach dessen Erstellung) nicht mehr verändert werden. D.h. Sie brauchen z.B. keine `set`ter Methoden programmieren.

## 1: Einfärbiges Bild

Für erste werden wir sehr einfache Bilder gestalten: Wir brauchen erstmal Farben: Farben werden wir "indiziert" darstellen, d.h. jede Farbe wird durch eine Ganzzahl beschrieben, und zusätzlich wird eine "Palette" festgelegt, die jeder Zahl/Index dann eine echte Farbe zuordnet:

__Anforderung:__ Ihr `Color` Typ soll eine indizierte Farbe darstellen.

Nachdem Sie nun über Farben verfügen, erstellen Sie ein komplett einfärbiges Bild:

```
Image filled = new FilledImage(new Color(12));
```
__Anforderung:__ Erstellen Sie einen Typ `FilledImage` der ein einfärbiges Bild beschreibt.

Nachdem wir nun ein Bild haben, brauchen wir noch eine Möglichkeit, uns dieses anzeigen zu lassen, im ersten Schritt werden wir eine Ausgabe auf der Konsole gestalten: Farben werden wir durch Großbuchstaben darstellen, die indizierte Farbe `0` wird dabei zu `'A'`, `1` zu `'B'`, ... `25` zu `'Z'`.
Das `Image` wird gesamplet, und für jedes Sample wird ein Buchstabe in einem Raster ausgegeben.
Zum Beispiel um das Bild im Bereich `x` = [`0`,`3`], `y` = [`1`, `2`] mit Samples im Abstand von jeweils `0.5` anzuzeigen verwenden wir folgenden Aufruf

```
Renderer renderer = new AsciiArtRenderer();
renderer.render(filled, 0, 1, 3, 2, 0.5);
```

Wodurch folgende Ausgabe zustande kommt:

```
MMMMMMM
MMMMMMM
MMMMMMM
```

durch jeweils 7 Samples pro Zeile (an den Positionen x = [`0`,`0.5`,`1`,`1.5`,`2`,`2.5`,`3`] und 3 Samples pro Spalte (an den Positionen y = [`1`,`1.5`,`2`]).

__Anforderung:__ Definieren Sie ein `interface Renderer` mit der Methode `render` so wie oben beschrieben.

__Anforderung:__ Implementieren Sie einen `AsciiArtRenderer` der eine Ausgabe so wie oben erstellt.

__Anforderung:__ Ihr Programm soll ein einfärbiges Bild zeichnen und auf der Konsole ausgeben.

## 2: Quadrat zeichnen

Einfärbige Bilder sind gut und schön aber auch etwas fad. Beginnen wir einfache Formen zu zeichnen: Versuchen wir uns zuerst an einem Quadrat, dieses wird durch eine Seitenlänge bestimmt und wird mittig platziert (Mittelpunkt auf `[0/0]`). Wir müssen dabei 2 Farben angeben, einmal für das Quadrat an sich und einmal für den Hintergrund:

```
Color squareColor = new Color(1);
Color backgroundColor = new Color(2);
double length = 3;
Image square = new SquareImage(length, squareColor, backgroundColor);
renderer.render(square, -3, -3, 3, 3, 1);
```

Erzeugt folgende Ausgabe
```
CCCCCCC
CCCCCCC
CCBBBCC
CCBBBCC
CCBBBCC
CCCCCCC
CCCCCCC
```
__Anforderung:__ Programmieren Sie `SquareImage` wie oben beschrieben.

__Anforderung:__ Ihr Programm soll ein Quadrat zeichnen und auf der Konsole ausgeben.

## 3: Komplexere Bilder

Um anspruchsvollere Bilder zu erzeugen, haben wir prinzipiell zwei Möglichkeiten:

1. Können wir anspruchsvollere Typen erzeugen, die komplexe Bilder für sich zeichnen
2. Können wir viele einfache Typen schreiben, und eine Möglichkeit zur Kombination bieten, um ein komplexes Bild aus vielen einfachen Bilder zusammenzustellen.

Für diese Übung werden wir die 2. Variante wählen. Wir bieten eine einfache Möglichkeit zur Kombination an, das _Übermalen_. D.h. wir können ein Bild herannehmen und dieses mit einem anderen Bild übermalen um ein neues, kombiniertes Bild zu erhalten.

```
Image background = ...
Image foreground = ...
Image combined = new LayeredImage(background, foreground);
```

D.h. wir erstellen ein neues Bild (`combined`) indem wir ein Bild (`background`) mit einem anderen (`foreground`) übermalen.

Ihnen ist wahrscheinlich aufgefallen, dass mit unseren bisherigen Mitteln, das Vordergrund Bild das Hintergrund Bild immer komplett übermalen muss. D.h. wir müssen auch hier nachbessern:

Sehen eine neue "Farbe" vor, die komplett _transparent_ ist. Wenn der Vordergrund transparent ist, scheint dann natürlich die Farbe aus dem Hintergrund durch.

__Anforderung__: Erstellen Sie einen Typ `LayeredImage` wie oben beschrieben.

__Anforderung__: Sehen sie eine neue "transparente Farbe" vor. Realisieren Sie das in Form einer __Klassenhirarchie__ (Ihre `Renderer` sollen eine `RuntimeException` werfen, sollten diese jemals eine "transparente Farbe" zeichnen müssen).

__Anforderung__: Vereinfachen Sie ihren Typ `SquareImage`, eine Hintergrundfarbe ist nicht mehr benötigt, da man ja zuerst ein `FilledImage` in der Hintergrundfarbe gestaltet und dass mit einem `SquareImage` übermalen kann.

__Hinweis:__ Denken Sie bei ihrem Design nach, welcher ihrer Typen welche Aufgabe übernimmt. Z.B. welcher Type entscheidet über die entstehende Farbe beim Übermalen?

Folgendes Bild müssten Sie nun Zeichnen können:

```
Image i;
i = new FilledImage(color0);
i = new LayeredImage(i, new SquareImage(3, color1));
i = new LayeredImage(i, new SquareImage(2, color2));
i = new LayeredImage(i, new SquareImage(1, color3));
renderer.render(i, -2, -2, 2, 2, 0.5);
```

Ausgabe:

```
AAAAAAAA
ABBBBBBA
ABCCCCBA
ABCDDCBA
ABCDDCBA
ABCCCCBA
ABBBBBBA
AAAAAAAA
```

## 4: Komfortablere API

Wie Sie vielleicht gemerkt haben, ist das erzeugen von Bilder mittlerweile schon recht umständlich. Zur Vereinfachung werden wir eine `Builder` Klasse erstellen, die diese Erstellung kapselt.

Das vorherige Bild lässt sich durch den `Builder` folgendermaßen erstellen:

```
Image i = new Builder()
    .fill(color0)
    .square(3, color1)
    .square(2, color2)
    .square(1, color3)
    .build();
```

__Anforderung:__ Programmieren Sie den `Builder` Typ wie oben beschrieben.

__Hinweis:__ Die Methoden `fill`, `square` und `build` sind alles Methoden der `class Builder`.

Wir können den `Builder` auch verwenden, um die ursprüngliche Funktionalität von `SquareImage` mit einer Quadrat- und Hintergrundfarbe bereitzustellen.

```
Image i = new Builder()
    .square(10, squareColor, backgroundColor)
    .build();
```

__Anforderung:__ Stellen Sie eine überladene `square` Methode bereit, die ein Quadrat mit Quadrat- und Hintergrundfarbe erstellt.

## 5: Transformierte Bilder

Neben Kombination können Bilder auch transformiert werden.

Die Transformationen die wir implementieren werden sind:

- Verschieben ("Translatieren")
- Vergrößern/Strecken und Verkleiner/Stauchen ("Skalieren")
- Drehen ("Rotieren")

Bilder sollen entlang der x und y Achsen verschoben werden können:

```
double xOffset = 5;
double yOffset = -3;
Image translated = new TranslatedImage(original, xOffset, yOffset);
```

Bilder sollen entlang der x und y Achse gestreckt und gestaucht werden können:

```
double xScale = 2;
double yScale = 0.5;
Image scaled = new ScaledImage(original, xScale, yScale);
```

Bilder können anhand eines Winkels (in Grad) im Uhrzeigersinn um den Koordinatenursprung (`[0/0]`) rotiert werden:

```
double angle = 45;
Image rotated = new RotatedImage(original, angle);
```

__Anforderung:__ Entwerfen Sie die Typen `TranslatedImage`, `ScaledImage` und `RotatedImage` so wie oben beschrieben.

__Hinweis:__  Ausgehend von einem Punkt `[x/y]` lautet der um den Winkel `w` im Uhrzeiger rotierte Punkt `[xr/yr]` wobei:

```
xr = x * cos(w) + y * sin(w)
yr = y * cos(w) - x * sin(w)
```

__Hinweis:__ Die `Math.cos` und `Math.sin` Funktionen von Java funktionieren mit Parametern in Radiant (und nicht in Grad). Verwenden Sie die Funktion `Math.toRadians` um von Grad in Radiant umzuwandeln.

Um die API einfach zu halten fügen Sie auch entsprechende Methoden zum `Builder` hinzu.

Ein Karo (so wie auf Spielkarten) können Sie dann so zeichnen:

```
Image diamonds = new Builder()
    .fill(color0)
    .square(10, color1)
    .rotate(45) // balance square on its tip
    .scale(0.5, 1) // squash it sideways
    .build();
```

__Anforderung:__ Erweitern Sie Ihren Builder um die Methoden `translate`, `scale`, `rotate` die so funktionieren wie oben.

Sie haben nun alles was sie brauchen um ein Rechteck zu zeichnen. Denken Sie nach wie.

__Anforderung:__ Erweitern Sie ihren `Builder` um eine Methode `rect` die es ihnen erlaubt ein beliebiges Rechteck an einer beliebigen Position zu zeichnen.

## 6: Echte Bilddateien

Darauf haben Sie sicher schon gewartet: Wir erstellen nun PNG-Dateien. Sie können dazu die Klassen `javax.imageio.ImageIO` und `java.awt.image.BufferedImage`  verwenden, etwa so:

```
int pixelWidth = 2;
int pixelHeight = 2;
BufferedImage bi = new BufferedImage(pixelWidth, pixelHeight, BufferedImage.TYPE_INT_RGB);
bi.setRGB(0, 0, 16711935); // [0,0] ...... coordinate
bi.setRGB(0, 1, 16711935); // 16711935 ... color pink
bi.setRGB(1, 0, 16711935);
bi.setRGB(1, 1, 16711935);
ImageIO.write(bi, "png", "output.png");
```

Das erstellt eine Datei `output.png` mit 4 Pixel (2 breit, 2 hoch), alle in rosa.

__Anforderung:__ Erstellen Sie einen alternativen `Renderer`, `PNGRenderer`, der eine Ausgabe in eine PNG-Datei erstellt.

__Anforderung:__ Ihr Programm soll ein Bild zeichnen und als PNG Datei ausgeben.

__Hinweis:__ Sie haben sich wahrscheinlich gefragt, wieso die Farbe rosa ausgerechnet `16711935` ist. Das funktioniert folgendermaßen: Im Computer müssen Sie Farben aus den Grundfarben _rot_, _grün_ und _blau_ mischen. Für die Mischung können Sie für jede dieser Grundfarben eine "Menge" von `0` bis `255` verwenden. `0` heißt "gar nicht verwenden" `255` heißt "so viel wie möglich verwenden". Die gemischte Farbe errechnet sich nach dieser Formel: `red * 256 * 256 + green * 256 + blue`, Z.b. für rosa mischt man so viel als möglich rot und blau aber gar kein grün, also `red = 255`, `blue = 255`, `green = 0`. Laut Formel ist die Mischfarbe dann `pink = 255 * 256 * 256 + 0 * 256 + 255` also `pink =  16711935`.

## Extra Aufgaben

Für den Fall dass Sie Ihr Programm noch weiter verbessern wollen, sind hier ein paar Vorschläge:

 - Weitere Bild-Bausteine
   - zB Kreise, Dreiecke, ...
 - Weitere Kombinationsmöglichkeiten
   - Informieren Sie sich über Bildmasken
 - Weitere Transformationen
   - Überlegen Sie sich z.B. "Instagram" Effekte einzubauen
