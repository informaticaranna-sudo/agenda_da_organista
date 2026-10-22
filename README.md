# 🎵 Organista no Ritmo

Aplicativo para organistas acompanharem o progresso de estudo: escalas, hinos, coros, métodos (Volumes 1 a 4) e fases do MSA. Cada item pode ser marcado com resultado, data e anotações.

> Tema escuro, interface em português (pt-BR).

## Funcionalidades

- **Login / cadastro** com tela de boas-vindas.
- **Tela Hoje**: resumo do dia e contadores de progresso por seção.
- **Listas por seção**: escalas, hinos, volumes e MSA, agrupadas por categoria.
- **Botão Marcar** em todos os itens, que abre a tela **Registrar progresso**.
- **Registrar progresso**: resultado (Passou / Não passou / Em avaliação), data de conclusão, anotações e histórico do item.
- **Navegação inferior**: Hoje, Progresso e Configurações.

## Catálogo de itens

| Seção | Conteúdo | Total |
|---|---|---|
| Escalas | 15 maiores + 15 menores | 30 |
| Hinos | Hinos 1 a 480 | 480 |
| Coros | Coros 1 a 5 | 5 |
| Volume 1 | 22 exercícios + 40 estudos + hinos 131, 158 e 468 | 65 |
| Volume 2 | 20 exercícios + 30 estudos | 50 |
| Volume 3 | 10 exercícios + 25 estudos | 35 |
| Volume 4 | 10 exercícios + 12 estudos | 22 |
| MSA | Fases 1 a 16 | 16 |

### Escalas

- **Maiores:** Dó, Sol, Ré, Lá, Mi, Si, Fá#, Dó#, Fá, Sib, Mib, Láb, Réb, Solb, Dób
- **Menores:** Lá, Mi, Si, Fá#, Dó#, Sol#, Ré#, Lá#, Ré, Sol, Dó, Fá, Sib, Mib, Láb

## Modelo de dados

```ts
type Item = {
  id: string;        // ex.: "escala-maior-Dó", "hino-131", "volume2-estudo-7"
  secao: string;     // escalas | hinos | volume1..4 | msa
  grupo: string;     // ex.: "Escalas Maiores", "Hinos", "Volume 1"
  titulo: string;
  subtitulo: string;
};

type Registro = {
  itemId: string;
  resultado: "passou" | "nao_passou" | "em_avaliacao";
  data: string;      // ISO 8601
  anotacoes: string;
};
```

## Geração do catálogo

```ts
const maiores = ["Dó","Sol","Ré","Lá","Mi","Si","Fá#","Dó#","Fá","Sib","Mib","Láb","Réb","Solb","Dób"];
const menores = ["Lá","Mi","Si","Fá#","Dó#","Sol#","Ré#","Lá#","Ré","Sol","Dó","Fá","Sib","Mib","Láb"];

const seq = (n: number, f: (i: number) => Item) =>
  Array.from({ length: n }, (_, i) => f(i + 1));

function volume(v: number, ex: number, es: number, hinos: number[] = []): Item[] {
  const s = `volume${v}`, g = `Volume ${v}`;
  return [
    ...seq(ex, i => ({ id: `${s}-exercicio-${i}`, secao: s, grupo: g, titulo: `Exercício ${i}`, subtitulo: g })),
    ...seq(es, i => ({ id: `${s}-estudo-${i}`, secao: s, grupo: g, titulo: `Estudo ${i}`, subtitulo: g })),
    ...hinos.map(h => ({ id: `${s}-hino-${h}`, secao: s, grupo: g, titulo: `Hino ${h}`, subtitulo: g })),
  ];
}

export const catalogo: Item[] = [
  ...maiores.map(n => ({ id: `escala-maior-${n}`, secao: "escalas", grupo: "Escalas Maiores", titulo: `${n} Maior`, subtitulo: "Escala Maior" })),
  ...menores.map(n => ({ id: `escala-menor-${n}`, secao: "escalas", grupo: "Escalas Menores", titulo: `${n} Menor`, subtitulo: "Escala Menor" })),
  ...seq(480, i => ({ id: `hino-${i}`, secao: "hinos", grupo: "Hinos", titulo: `Hino ${i}`, subtitulo: "Hino" })),
  ...seq(5, i => ({ id: `coro-${i}`, secao: "hinos", grupo: "Coros", titulo: `Coro ${i}`, subtitulo: "Coro" })),
  ...volume(1, 22, 40, [131, 158, 468]),
  ...volume(2, 20, 30),
  ...volume(3, 10, 25),
  ...volume(4, 10, 12),
  ...seq(16, i => ({ id: `msa-${i}`, secao: "msa", grupo: "MSA", titulo: `Fase ${i}`, subtitulo: "MSA" })),
];
```

## Regras de progresso

1. Cada card da tela **Hoje** abre a lista da sua seção, agrupada por `grupo`.
2. O botão **Marcar** abre **Registrar progresso** com o `itemId` correspondente.
3. Ao salvar, grava-se um `Registro`; o histórico mostra apenas os registros do item.
4. Os contadores (ex.: `0/30`, `0/480`) contam itens com ao menos um registro **Passou**.

## Como rodar

_Preencha conforme a tecnologia do projeto (instalação, execução e build)._

```bash
# exemplo
git clone https://github.com/SEU-USUARIO/organista-no-ritmo.git
cd organista-no-ritmo
```

## Licença

Defina a licença do projeto (por exemplo, MIT).
