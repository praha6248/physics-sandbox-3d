# Sandbox — symulacja spadających kul z różnych materiałów

Prosty silnik fizyczny napisany od zera w JavaScripcie i wizualizowany w 3D w przeglądarce.
Na scenę wrzuca się kule ze stali, gumy albo styropianu i obserwuje, jak spadają, odbijają się,
zderzają ze sobą i — w przypadku styropianu — rozpadają przy mocnym uderzeniu.
Grawitację można przełączać między Ziemią, Księżycem, Marsem i Słońcem.

<img width="3501" height="1629" alt="Zrzut ekranu symulacji" src="https://github.com/user-attachments/assets/7151c3ae-1ac7-4c94-9e89-b2b0c2949946" />

![React](https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=black)
![Three.js](https://img.shields.io/badge/Three.js-r182-000000?logo=threedotjs)
![Vite](https://img.shields.io/badge/Vite-7-646CFF?logo=vite&logoColor=white)

## Funkcje

- **Trzy materiały** o różnych parametrach fizycznych:

  | Materiał | Gęstość [kg/m³] | Współczynnik odbicia | Tarcie | Promień |
  |---|---|---|---|---|
  | Stal | 7500 | 0.3 | 0.4 | 1.0 |
  | Guma | 1300 | 0.9 | 0.9 | 0.5 |
  | Styropian | 20 | 0.1 | 0.5 | 1.5 |

- **Zmienna grawitacja** — Ziemia (9,81 m/s²), Księżyc (1,62), Mars (3,73), Słońce (274).
- **Zderzenia kula–kula** — rozwiązywane impulsowo z uwzględnieniem mas, z rozsuwaniem nakładających się kul.
- **Odbicia od podłoża** z tłumieniem i tarciem zależnym od materiału.
- **Kruszenie** — lekkie kule (styropian) uderzone z prędkością > 12 m/s (o podłoże albo inną kulę) rozpadają się na trzy mniejsze
  fragmenty o mniejszym promieniu i masie.
- **Kamera orbitalna** — obracanie, przybliżanie i przesuwanie widoku myszą.

## Jak to działa

Pętla fizyki działa w `useFrame` (co klatkę, krok `dt = 0,016 s`):

1. całkowanie Eulera: `v += g·dt`, `x += v·dt`,
2. kolizja z podłożem — odbicie ze współczynnikiem restytucji, tarcie poziome, ewentualne kruszenie,
3. kolizje między parami kul — impuls wzdłuż normalnej zderzenia, korekta położenia,
4. usunięcie rozbitych kul i dodanie fragmentów.

Masa kuli liczona jest z objętości i gęstości materiału: `m = 4/3·π·r³·ρ`.

## Uruchomienie

Wymagany Node.js 20+.

```bash
npm install        # albo: yarn
npm run dev        # http://localhost:5173
```

```bash
npm run build      # build produkcyjny do dist/
npm run preview    # podgląd builda
```

## Struktura

```
src/
  App.jsx           definicje materiałów i grawitacji, tworzenie kul, scena (Canvas, światło, podłoże)
  PhysicsScene.jsx  silnik fizyki: całkowanie, kolizje, kruszenie, renderowanie kul
  UI.jsx            panel z przyciskami materiałów i wyboru grawitacji
```

## Technologie

React 19 · Three.js · @react-three/fiber · @react-three/drei · Vite
