﻿# banco-de-dados-2
DELIMITER //
CREATE PROCEDURE sp_LivrosPorCategoria(IN nome_categoria VARCHAR(50))
BEGIN
	SELECT l.Titulo, c.Nome AS Categoria
    FROM Livro l
    INNER JOIN Categoria c ON li.Categoria_ID = c.Categoria_ID
    WHERE c.Nome = nome_categoria;
END;
