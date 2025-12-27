---
publish: false
---
# 🧙‍♀️ Lista Mestra de Personagens (NPCs)

Este painel mostra todos os personagens relevantes, ordenados por Grau de Ameaça.

## Tabela de NPCs por Grau de Ameaça

```dataview
TABLE 
    categoria AS "Tipo",
    grau AS "Grau",
    status AS "Status",
    grupo AS "Facção"
FROM "02_NPCs" 
WHERE tipo = "Personagem"
SORT choice(grau = "Especial", 0, choice(number(grau), number(grau), 10)) ASC
```

---

## 🤝 Rastreamento: Quem tem Relação com um NPC Chave

> 💡 Para rastrear um NPC, mude o link interno no filtro `[[Mahito]]`

```dataview
TABLE WITHOUT ID
    file.link AS "NPC Relacionado",
    grau AS "Grau",
    status AS "Status",
    grupo AS "Afiliação"
FROM "02_NPCs" 
WHERE 
    tipo = "Personagem" AND 
    contains(relacoes, [[Uli Kamo]])
SORT file.link ASC
```




