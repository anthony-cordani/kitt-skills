# French AI tells

Load this file whenever the text is in French. It adds to the patterns in [SKILL.md](../SKILL.md), it does not replace them: the structural tells there (§1 contrasts, §2 closers, §6 triads, §19 bold labels, §22 chatbot residue, §26 re-explaining) show up in French as often as in English. The patterns below are the ones French output adds or changes.

Numbering uses an F prefix so it never collides with the § numbers in SKILL.md. Patterns run strongest first. One sighting of F1 to F5 justifies an edit; a pattern marked *weak alone* needs other tells in the same passage.

Register matters more in French than in English. A formal letter to an administration and an SMS to a friend follow different rules, and several patterns below are normal in the first and a tell in the second. Judge each one against the relationship and the channel.

## A. Stock wrappers

### F1. Stock closers

**Watch for:** N'hésitez pas à (me contacter, revenir vers moi, me faire part de…); Je reste à votre (entière) disposition pour toute question; Au plaisir d'échanger; Bonne journée à vous !; Belle journée !; En espérant que cela vous aide; Dites-moi si vous souhaitez que je…
**Problem:** The model closes every message with an open invitation, whatever the message says. "Je reste à votre disposition" is ordinary at the end of a formal business email, so it is weak there; in a chat message, a review reply or a note to someone the writer knows, it reads like a template. Cut the closer, or replace it with the one concrete next step the reader can take.
**Before:**
> Votre commande est partie ce matin. N'hésitez pas à revenir vers moi si vous avez la moindre question, je reste à votre entière disposition. Bonne journée à vous !
**After:**
> Votre commande est partie ce matin.

### F2. Stock openers

**Watch for:** J'espère que ce message vous trouve bien (calque of "I hope this message finds you well"); J'espère que vous allez bien (as a reflex, to someone the writer has no news from); Je me permets de vous contacter afin de; Merci pour votre message !; Je comprends tout à fait votre frustration
**Problem:** The first line delays the point with a ritual. "J'espère que ce message vous trouve bien" is a direct translation and almost no French writer uses it, so it is a strong tell on one sighting. The others are weak alone. Start with the reason for writing.
**Before:**
> Bonjour Claire, j'espère que ce message vous trouve bien. Je me permets de vous contacter afin de faire le point sur le devis.
**After:**
> Bonjour Claire, je reviens vers vous pour le devis.

### F3. Ce n'est pas X, c'est Y

**Watch for:** ce n'est pas seulement X, c'est Y; il ne s'agit pas de X mais de Y; plus qu'un X, un véritable Y; non pas X, mais Y; the split form "Ce n'est pas une question de prix. C'est une question de confiance."
**Problem:** The French form of §1. The negative half rejects something nobody said, so the positive half sounds bigger. State Y directly, unless the reader really believes X.
**Before:**
> Plus qu'une simple application, c'est un véritable compagnon du quotidien : chaque semaine, elle vous propose des recettes. Il ne s'agit pas de cuisiner plus, mais de cuisiner mieux.
**After:**
> L'appli vous propose des recettes chaque semaine.

## B. Stock words and inflation

### F4. Stock connectors

**Watch for:** Il est important de noter que; Il convient de souligner que; Force est de constater que; En somme; En définitive; Par ailleurs and De plus opening consecutive sentences; Ainsi, opening a conclusion; Il est à noter que
**Problem:** These announce that a point matters instead of making it, and models chain them far more than people do. A French writer uses a connector when the logic needs one. Remove the connector and keep the sentence; if the sentence then has nothing to say, remove it too.
**Before:**
> Il est important de noter que le délai de livraison est de cinq jours. Par ailleurs, les frais de port sont offerts. En somme, c'est une offre avantageuse.
**After:**
> La livraison prend cinq jours et le port est offert.

### F5. Inflated words

**Watch for:** véritable, incontournable, crucial, essentiel, primordial, au cœur de, s'inscrire dans (une démarche, une dynamique), dans un monde où, à l'ère de, un levier, un enjeu majeur, une approche holistique, sublimer, révolutionner, optimiser (figurative), une expérience unique
**Problem:** The French counterpart of §12 and §13. Each word inflates an ordinary fact, and they come in groups. "Dans un monde où…" as an opening line is a strong tell on one sighting. Replace the word with the fact it was dressing up.
**Before:**
> Dans un monde où le temps est devenu une ressource incontournable, notre service de livraison en 24 h s'inscrit dans une démarche visant à placer le client au cœur de l'expérience.
**After:**
> On livre en 24 h.

## C. Rhythm and punctuation

### F6. Em dashes as connectors

**Watch for:** "—" between two clauses, around an aside, or before a final punchline; the spaced en dash " – " used the same way
**Problem:** French prose rarely uses the tiret cadratin as a connector. It marks dialogue and the occasional incise in edited text, not a clause in an email or a text message. A model trained mostly on English puts one in every other sentence. Replace it with a comma, parentheses, a colon, or two sentences. §8 applies in full: no dashes in the final text unless the writer's sample uses them. One dash in formal prose is *weak alone*.
**Before:**
> J'ai regardé votre dossier — tout est en ordre — et je l'envoie demain au comptable — sauf contre-ordre de votre part.
**After:**
> J'ai regardé votre dossier, tout est en ordre. Je l'envoie demain au comptable, sauf si vous me dites d'attendre.

### F7. Forced triads

**Watch for:** three adjectives or nouns in a row (simple, rapide et efficace; écoute, bienveillance et professionnalisme); three parallel short sentences followed by a lesson
**Problem:** The French form of §6. Keep every distinct claim supported by the source; cut only padding items that add no separate idea, and simplify the wording or structure. In the review reply below, "rapide, fiable et chaleureux" is unsupported padding.
**Before:**
> Merci pour votre avis ! Ravis que le gâteau soit arrivé à temps pour l'anniversaire. Notre équipe met un point d'honneur à offrir un service rapide, fiable et chaleureux.
**After:**
> Merci pour votre avis, ravis que le gâteau soit arrivé à temps pour l'anniversaire.

### F8. Exclamation marks and emojis by rule

**Watch for:** an exclamation mark on every sentence of a reply; one emoji per line or per bullet; 🙏 😊 🚀 ✨ added to a professional message; a "!" after a greeting and another after the sign-off
**Problem:** The model applies enthusiasm evenly. People use exclamation marks and emojis where they feel something, and their habits depend on the relationship. Keep at most the ones the writer would use with this person (the voice profile or sample decides); otherwise remove them.
**Before:**
> Super nouvelle ! 🎉 Votre rendez-vous est confirmé pour mardi 14 h ! On a hâte de vous voir ! 😊
**After:**
> C'est confirmé pour mardi 14 h, à bientôt.

## D. Formatting and politeness

### F9. Bold and lists inside a short message

**Watch for:** a three-line email or SMS with bold words; bullet points listing two items; a "**Prochaines étapes :**" label in a chat message; a heading in an email
**Problem:** The French form of §19, worse in messages, because nobody bolds words in a text to a client or a friend. Write the message as sentences. Keep a list only when the reader must act on several separate items, and even then without bold labels.
**Before:**
> Bonjour Marc,
> - **Date :** jeudi 12
> - **Heure :** 10 h
> - **Lieu :** à l'atelier
**After:**
> Bonjour Marc, rendez-vous jeudi 12 à 10 h à l'atelier.

### F10. Over-politeness and stacked thanks

**Watch for:** Merci beaucoup pour votre message, merci pour votre patience et merci encore pour votre confiance; Nous sommes sincèrement désolés pour la gêne occasionnée; je vous remercie par avance; Encore merci !; apologies and thanks in the first and last line
**Problem:** The model thanks and apologises by reflex, often twice. One thank-you at the right place is enough. In a review reply, one apology plus what was done about the problem reads as sincere; three apologies read as a template.
**Before:**
> Merci infiniment pour votre retour et pour le temps que vous avez pris pour nous écrire. Nous sommes sincèrement désolés pour la gêne occasionnée par le bug de synchronisation, qui est corrigé dans la version 2.4 sortie hier. Encore merci pour votre patience !
**After:**
> Désolé pour le bug de synchronisation, il est corrigé dans la version 2.4 sortie hier.

### F11. "Vous" or "tu" that does not fit the relationship

**Watch for:** "vous" to a friend, a family member or a contact who already uses "tu" with the writer; "tu" to a client or an app-store reviewer when the brand speaks with "vous"; inconsistent use of both forms for the same single person
**Problem:** The model defaults to "vous" because it is safe, or copies the "tu" of a casual prompt into a formal reply. A French reader notices at once. Use the form the writer already uses with this person: check the thread, the voice profile, or ask. Inconsistent address of the same single person is an error; "vous" may address a group that includes someone addressed as "tu".
**Before:**
> Salut Julien ! Merci pour votre message, je vous envoie les photos ce soir.
**After:**
> Salut Julien, je t'envoie les photos ce soir.

## E. Translation residue

### F12. Anglicisms and calques

**Watch for:** faire du sens (make sense; use "avoir du sens" or "c'est logique"); adresser un problème (address; use "traiter", "régler"); en termes de used as reflex filler ("en termes de délais", "en termes de qualité" stacked in a short message; use "pour", "côté", or rephrase); impacter (impact, as a reflex); être en charge de (be in charge of; "s'occuper de", "être responsable de"); définitivement for "definitely" (use "vraiment", "clairement"); opportunité for "chance"; supporter for "soutenir"; à la fin de la journée (at the end of the day; "au final"); délivrer de la valeur
**Problem:** These can come from text generated in English and translated, or from a model thinking in English. "En termes de" is accepted in terminology ("en termes juridiques", "en termes de droit") and commonly used to mean "sur le plan de"; flag only its use as reflex filler, not the expression itself. Some listed expressions have entered business French, so one of them is *weak alone*; two or three reflex uses in a short message point to machine output. Use the French expression or simplify the filler.
**Before:**
> En termes de délais, ça fait du sens d'adresser ce problème avant la fin du mois.
**After:**
> Pour les délais, mieux vaut régler ça avant la fin du mois.

### F13. Machine-perfect typography in a casual register

**Watch for:** a non-breaking space before every ? ! : ; in a text message; « guillemets » with inner spaces in a chat; the correct ellipsis character "…" and the typographic apostrophe "’" everywhere in an SMS; perfect accents on capitals (État, À) in a quick reply
**Problem:** These rules are correct French typography, and in formal text or an edited document they are expected, not a tell. In an SMS or chat, people type with a phone keyboard and rarely produce them so consistently. *Weak alone*, and never a reason to introduce errors: when other tells point to machine output, match the writer's usual typing habits (the voice profile or sample), otherwise leave the typography alone.
**Before (SMS to a friend, with non-breaking spaces and curly apostrophes):**
> Tu passes à quelle heure ? J’ai prévu « un truc » pour ce soir !
**After (as the writer types):**
> Tu passes à quelle heure? J'ai prévu un truc pour ce soir
