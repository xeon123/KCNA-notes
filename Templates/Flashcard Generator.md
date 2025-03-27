<%*
const dv = app.plugins.plugins["dataview"].api;
const notes = dv.pages("").where(p => p.flashcard);
notes.forEach(n => { %>
Q: <%= n.Q %>
A: ==<%= n.A %>==
<!--SR:!2025-03-30,4,270-->

<% }) %>