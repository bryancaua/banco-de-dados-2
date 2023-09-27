# banco-de-dados-2
DELIMITER //
CREATE PROCEDURE sp_LivrosPorCategoria(IN nome_categoria VARCHAR(50))
BEGIN
	SELECT l.Titulo, c.Nome AS Categoria
    FROM Livro l
    INNER JOIN Categoria c ON li.Categoria_ID = c.Categoria_ID
    WHERE ca.Nome = nome_categoria;
END;

-- exercício 3
DELIMITER //
CREATE PROCEDURE sp_ContarLivrosPorCategoria(IN nome_categoria VARCHAR(225))
BEGIN
	SELECT ca.Nome, COUNT(l.Titulo) AS quantidade_livros
    FROM Livro l
    INNER JOIN Categoria c ON li.Categoria_ID = c.Categoria_ID
    WHERE ca.Nome = nome_categoria
    GROUP BY ca.Nome;
END;
