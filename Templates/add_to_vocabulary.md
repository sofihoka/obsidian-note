<%* 
//EJECUTAR COMMAND + p 
//
// se van a agregar automaticamente las palabras con :: no si si la flashcard me la 
//Agregar automatico a las flashcard
//1.poner palabra:: significado
//2.atajo command + p
//3.open template insert modal
//4.elegir template add_to_vocabulary

// Pedir la palabra al usuario
    let word = await tp.system.prompt("Enter the word:");
    
    // Si el usuario no ingresa nada, salir
    if (!word) return "";
    
const vocabPath = "vocabulary/vocabulary.md"; 

// Obtener el archivo actual
const currentFilePath = tp.file.path(true);
const currentFile = app.vault.getAbstractFileByPath(currentFilePath);

if (!currentFile) {
    new Notice(currentFilePath+"❌ Error: No se pudo encontrar el archivo actual.");
    return;
}

// Leer el contenido del archivo actual
const content = await app.vault.read(currentFile); 

// Filtrar solo las líneas que contienen "::"
const vocabLines = content.split("\n").filter(line => line.includes("::")||line.includes(":::"));

if (vocabLines.length === 0) {
    new Notice("ℹ️ No se encontraron líneas con '::' en este archivo.");
    return;
}

// Obtener el archivo de vocabulario principal
const vocabFile = app.vault.getAbstractFileByPath(vocabPath);
if (!vocabFile) {
    new Notice("❌ Error: No se encontró 'vocabulary.md'. Verifica la ruta.");
    return;
}

// Leer el contenido actual del archivo principal
const mainVocab = await app.vault.read(vocabFile); 

// Agregar las nuevas líneas sin duplicados
const updatedVocab = new Set([...mainVocab.split("\n"), ...vocabLines]);
await app.vault.modify(vocabFile, Array.from(updatedVocab).join("\n"));

// Notificación de éxito
new Notice("✅ Vocabulario agregado correctamente.");

module.exports = async function(tp) {
    return await tp.system.prompt("Enter the word:");
};
%>
