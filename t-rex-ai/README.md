# T-Rex AI

> Chrome's offline T-Rex easter egg, automatizado con Algoritmos Genéticos y Redes Neuronales.

## Instalación y Ejecución

```bash
npm install
npm run dev       # http://localhost:5173
npm run build
```

| Key | Action |
|-----|--------|
| `Space` / `↑` | Jump |
| `↓` | Duck |
| `Enter` | Restart after crash |

See [`t-rex-runner/README.md`](t-rex-runner/README.md) for full details.

---

## t-rex-ai

Agente de Inteligencia Artificial que aprende a jugar T-Rex Runner autónomamente utilizando Redes Neuronales y un Algoritmo Genético.

En esta implementación, todo el procesamiento y el entrenamiento ocurre de manera 100% nativa **localmente en el navegador** utilizando Typescript y Neuroevolución en memoria RAM.

La IA toma las decisiones de saltar, caer rápido o agacharse a través de una red neuronal customizada (sin frameworks pesados), evaluando 5 parámetros físicos del entorno.

---

## Credits

- Original game: [Chromium Authors](https://chromium.googlesource.com/chromium/src/+/main/components/neterror/resources/) — BSD 3-Clause.
- Standalone extraction: [wayou/t-rex-runner](https://github.com/wayou/t-rex-runner).
- Vite + TypeScript port & RL extension: [Aldair Andrade](https://github.com/aymefreyson), 2026.

## License

BSD 3-Clause — see [LICENSE](LICENSE).
