# titulo_dinamico_pbi
Este projeto contém um script DAX que gera títulos dinâmicos para dashboards no Power BI, com base nos filtros aplicados pelo usuário. O objetivo é tornar os relatórios mais informativos e personalizados, refletindo os critérios de seleção diretamente no cabeçalho do dashboard.

// Captura os valores selecionados, tratando múltiplas seleções
VAR VAR1 = 
    IF(ISFILTERED(Tabela1[Coluna1]), 
       IF(HASONEVALUE(Tabela1[Coluna1]), 
          SELECTEDVALUE(Tabela1[Coluna1]), 
          "Vários Valores 1"), 
       BLANK())

VAR VAR2 = 
    IF(ISFILTERED(Tabela2[Coluna2]), 
       IF(HASONEVALUE(Tabela2[Coluna2]), 
          SELECTEDVALUE(Tabela2[Coluna2]), 
          "Vários Valores 2"), 
       BLANK())

VAR VAR3 = 
    IF(ISFILTERED(Tabela2[Coluna3]), 
       IF(HASONEVALUE(Tabela2[Coluna3]), 
          SELECTEDVALUE(Tabela2[Coluna3]), 
          "Vários Valores 3"), 
       BLANK())

VAR VAR4 = 
    IF(ISFILTERED(Tabela2[Coluna4]), 
       IF(HASONEVALUE(Tabela2[Coluna4]), 
          SELECTEDVALUE(Tabela2[Coluna4]), 
          "Vários Valores 4"), 
       BLANK())

VAR VAR5 = 
    IF(ISFILTERED(Tabela3[Coluna5]), 
       IF(HASONEVALUE(Tabela3[Coluna5]), 
          SELECTEDVALUE(Tabela3[Coluna5]), 
          "Vários Valores 5"), 
       BLANK())

VAR VAR6 = 
    IF(ISFILTERED(Tabela4[Coluna6]), 
       IF(HASONEVALUE(Tabela4[Coluna6]), 
          SELECTEDVALUE(Tabela4[Coluna6]), 
          "Vários Valores 6"), 
       BLANK())

// Cria uma tabela com os textos de cada variável, se estiverem preenchidas
VAR PARTES = 
    ADDCOLUMNS(
        {
            ( "Filtro 1", VAR1 ),
            ( "Filtro 2", VAR2 ),
            ( "Filtro 3", VAR3 ),
            ( "Filtro 4", VAR4 ),
            ( "Filtro 5", VAR5 ),
            ( "Filtro 6", VAR6 )
        },
        "Texto", IF(NOT ISBLANK([Value2]), [Value1] & ": " & [Value2], BLANK())
    )

// Filtra apenas os textos não em branco
VAR PARTES_VALIDAS = FILTER(PARTES, NOT ISBLANK([Texto]))

// Verifica se há pelo menos um filtro aplicado
VAR TEM_FILTRO = COUNTROWS(PARTES_VALIDAS) > 0

// Define o título padrão
VAR TITULO_PADRAO = "Título Padrão do Relatório"

// Se houver qualquer filtro, monta o título com prefixo
VAR TITULO_FINAL = 
    IF(
        TEM_FILTRO,
        "Título com Filtros " & CONCATENATEX(PARTES_VALIDAS, [Texto], " | "),
        TITULO_PADRAO
    )

RETURN TITULO_FINAL
