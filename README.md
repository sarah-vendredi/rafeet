# Guess the Feet 🦶

Quiz game show : devine ce que font les pieds de Raphaël.

## Lancer le projet

```bash
npm install
npm run dev
```

Ouvre http://localhost:5173

## Ajouter des vidéos

Place tes vidéos dans `public/videos/` avec le format `01.mp4`, `02.mp4`, etc.

## Extraire les posters (première frame)

```bash
ffmpeg -i public/videos/01.mp4 -vframes 1 -q:v 2 public/posters/01.jpg
```

Pour traiter toutes les vidéos en masse :

```bash
for i in $(seq -f "%02g" 1 23); do
  ffmpeg -i public/videos/${i}.mp4 -vframes 1 -q:v 2 public/posters/${i}.jpg -y
done
```

> Note : sans ffmpeg, l'app utilise directement la vidéo pausée sur la première frame — ça marche très bien.

## Remplir les questions (`src/data/rounds.ts`)

Chaque round suit cette structure :

```ts
{
  id: 1,
  video: '/videos/01.mp4',
  question: 'Où vont les pieds de Raphaël ?',
  answers: [
    { label: 'A', text: 'Sauter dans une piscine' },
    { label: 'B', text: 'Grimper une falaise' },
    { label: 'C', text: 'Traverser un marché' },
    { label: 'D', text: 'Courir un marathon' },
  ],
  correctAnswer: 'A',  // ← la bonne réponse
}
```

Les 23 rounds sont pré-remplis avec des exemples à remplacer par les vraies réponses.

## Structure

```
public/
  videos/   ← 01.mp4 … 23.mp4
  posters/  ← 01.jpg … 23.jpg (optionnel)
src/
  components/
    GameIntro.vue      ← écran d'accueil
    GameRound.vue      ← orchestrateur du round (state machine)
    PosterFrame.vue    ← image de freeze frame
    QuestionBlock.vue  ← titre de la question
    AnswerGrid.vue     ← 4 boutons A/B/C/D
    VideoReveal.vue    ← lecteur vidéo de révélation
    GameResults.vue    ← écran de score final
  data/
    rounds.ts          ← les 23 rounds
```
