---
title: Hermes Agent OS + Obsidian
created: 2026-06-22
updated: 2026-06-22
type: concept
tags: [hermes, obsidian, agent-os, memory, knowledge-base]
sources:
  - raw/videos/hermes-knowledge-base-self-improves-2026-06-14.md
  - https://www.youtube.com/watch?v=ipYz0hFa8qA
confidence: medium
---

# Hermes Agent OS + Obsidian

Το σωστό setup δεν είναι «ένα chatbot». Είναι **σύστημα**:

- **Hermes** = ο agent που τρέχει τη δουλειά.
- **Obsidian** = η μόνιμη μνήμη / knowledge base.
- **Skills** = επαναχρησιμοποιήσιμες διαδικασίες.
- **LLM Wiki** = εξωτερική γνώση που μπορεί να διαβάζει ο Hermes.

## Τι σημαίνει πρακτικά
Όταν ο Δημήτρης δίνει μια νέα πηγή στον Hermes, το σύστημα πρέπει να κάνει τα εξής:

1. Να αποθηκεύει το **raw source** σε `raw/`.
2. Να φτιάχνει ένα καθαρό **concept note**.
3. Να συνδέει το νέο note με υπάρχοντα notes με `[[wikilinks]]`.
4. Να ενημερώνει related workflows / skills όταν κάτι επαναλαμβάνεται.
5. Να κρατάει το vault καθαρό: raw data ξεχωριστά από distilled knowledge.

## Γιατί αξίζει
- Δεν χάνεται context.
- Οι αποφάσεις γίνονται επαναχρησιμοποιήσιμες.
- Ο Hermes μπορεί να δουλεύει πάνω στην ίδια γνώση με άλλα εργαλεία.
- Το setup μεγαλώνει με τον καιρό αντί να ξαναρχίζει από το μηδέν κάθε φορά.

## Related
- [[concepts/hermes-memory]]
- [[concepts/llm-wiki]]
- [[concepts/self-improving-knowledge-base]]
- [[Hermes/Operations/Hermes Agent OS + Obsidian]]
