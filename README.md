- 1.procédure ajout_produit
- 
CREATE OR REPLACE PROCEDURE ajout_produit (
    p_id_produit      IN NUMBER,
    p_nom             IN VARCHAR2,
    p_description     IN VARCHAR2,
    p_quantite        IN NUMBER,
    p_prix            IN NUMBER,
    p_id_categorie    IN NUMBER,
    p_id_localisation IN NUMBER
) 
IS
BEGIN
    INSERT INTO PRODUIT (
        id_produit, 
        nom, 
        description, 
        quantite, 
        prix, 
        id_categorie, 
        id_localisation
    ) VALUES (
        p_id_produit, 
        p_nom, 
        p_description, 
        p_quantite, 
        p_prix, 
        p_id_categorie, 
        p_id_localisation
    );

    COMMIT;
    
    DBMS_OUTPUT.PUT_LINE('Le produit ' || p_nom || ' a été ajouté avec succès.');


END;
/

<img width="1710" height="881" alt="Screenshot 2025-11-27 at 8 37 59 PM" src="https://github.com/user-attachments/assets/4f30ed03-a266-42e5-a760-fd844e3ceca3" />

<img width="1710" height="881" alt="Screenshot 2025-11-27 at 9 32 36 PM" src="https://github.com/user-attachments/assets/1a418c36-f12d-4368-8e7f-0bfb684b229a" />

BEGIN
    ajout_produit(
        p_id_produit      => &Saisir_ID_Produit,
        p_nom             => '&Saisir_Nom_Produit',      
        p_description     => '&Saisir_Description',       
        p_quantite        => &Saisir_Quantite,
        p_prix            => &Saisir_Prix,
        p_id_categorie    => &Saisir_ID_Categorie,       
        p_id_localisation => &Saisir_ID_Localisation      
    );
END;
/


<img width="1710" height="881" alt="Screenshot 2025-11-27 at 9 33 16 PM" src="https://github.com/user-attachments/assets/8a49d624-4101-4e38-b27a-fabbad52d23d" />


<img width="1710" height="881" alt="Screenshot 2025-11-27 at 9 34 51 PM" src="https://github.com/user-attachments/assets/c34fdd89-59e5-472f-8c7e-157926b9071a" />


CREATE OR REPLACE PROCEDURE ajout_produit (
    p_id_produit      IN NUMBER,
    p_nom             IN VARCHAR2,
    p_description     IN VARCHAR2,
    p_quantite        IN NUMBER,
    p_prix            IN NUMBER,
    p_id_categorie    IN NUMBER,
    p_id_localisation IN NUMBER
) 
IS
    prix_invalide EXCEPTION;
BEGIN
    IF p_prix < 0 THEN
        RAISE prix_invalide;
    END IF;

    INSERT INTO PRODUIT (
        id_produit, 
        nom, 
        description, 
        quantite, 
        prix, 
        id_categorie, 
        id_localisation
    ) VALUES (
        p_id_produit, 
        p_nom, 
        p_description, 
        p_quantite, 
        p_prix, 
        p_id_categorie, 
        p_id_localisation
    );

    COMMIT;
    DBMS_OUTPUT.PUT_LINE('Le produit ' || p_nom || ' a été ajouté avec succès.');

EXCEPTION
    WHEN prix_invalide THEN
        ROLLBACK; 
        DBMS_OUTPUT.PUT_LINE('ERREUR : Le prix ne peut pas être négatif.');
END;
/

<img width="1710" height="881" alt="Screenshot 2025-11-27 at 9 45 25 PM" src="https://github.com/user-attachments/assets/e2a37ed6-03f1-49df-85a1-960f3602c5a5" />
<img width="1710" height="881" alt="Screenshot 2025-11-27 at 9 45 54 PM" src="https://github.com/user-attachments/assets/82a6093a-fd01-4e2b-9f54-9029c869e961" />
<img width="1710" height="881" alt="Screenshot 2025-11-27 at 9 46 52 PM" src="https://github.com/user-attachments/assets/af241371-425b-4e73-a34d-f61504691b26" />


3 - 

CREATE OR REPLACE FUNCTION mise_ajour_produit (
    p_id_produit IN NUMBER,
    p_choix      IN NUMBER,    
    p_valeur     IN VARCHAR2   
) RETURN PRODUIT%ROWTYPE      
IS
    v_produit_ajour PRODUIT%ROWTYPE;
BEGIN
    CASE p_choix
        WHEN 1 THEN 
            UPDATE PRODUIT SET nom = p_valeur 
            WHERE id_produit = p_id_produit;
            
        WHEN 2 THEN 
            UPDATE PRODUIT SET description = p_valeur 
            WHERE id_produit = p_id_produit;
            
        WHEN 3 THEN 
            UPDATE PRODUIT SET prix = TO_NUMBER(p_valeur) 
            WHERE id_produit = p_id_produit;
            
        WHEN 4 THEN 
            UPDATE PRODUIT SET quantite = TO_NUMBER(p_valeur) 
            WHERE id_produit = p_id_produit;
            
        WHEN 5 THEN 
            UPDATE PRODUIT SET id_categorie = TO_NUMBER(p_valeur) 
            WHERE id_produit = p_id_produit;
            
        WHEN 6 THEN
            UPDATE PRODUIT SET id_localisation = TO_NUMBER(p_valeur) 
            WHERE id_produit = p_id_produit;
            
    END CASE;

    COMMIT;

    SELECT * INTO v_produit_ajour 
    FROM PRODUIT 
    WHERE id_produit = p_id_produit;

    RETURN v_produit_ajour;

END;
/


DECLARE
    v_resultat PRODUIT%ROWTYPE;
    v_choix    NUMBER;
    v_valeur   VARCHAR2(100);
    v_id       NUMBER;
BEGIN
    DBMS_OUTPUT.PUT_LINE('--- MENU MODIFICATION ---');
    DBMS_OUTPUT.PUT_LINE('1. Nom | 2. Description | 3. Prix | 4. Quantité | 5. Catégorie | 6. Localisation');
    
    v_id     := &Saisir_ID_Produit;
    v_choix  := &Saisir_Numero_Champ;
    v_valeur := '&Saisir_Nouvelle_Valeur'; -- Attention aux guillemets pour le texte

    v_resultat := mise_ajour_produit(v_id, v_choix, v_valeur);

    DBMS_OUTPUT.PUT_LINE('--------------------------------');
    DBMS_OUTPUT.PUT_LINE('MISE A JOUR REUSSIE :');
    DBMS_OUTPUT.PUT_LINE('Nouveau Nom   : ' || v_resultat.nom);
    DBMS_OUTPUT.PUT_LINE('Nouveau Prix  : ' || v_resultat.prix);
    DBMS_OUTPUT.PUT_LINE('Nouvelle Qté  : ' || v_resultat.quantite);
    DBMS_OUTPUT.PUT_LINE('Description   : ' || v_resultat.description);
END;
/
<img width="1710" height="881" alt="Screenshot 2025-11-27 at 10 09 13 PM" src="https://github.com/user-attachments/assets/914a7e81-9966-42df-ae27-497d62554800" />


<img width="1710" height="881" alt="Screenshot 2025-11-27 at 10 07 02 PM" src="https://github.com/user-attachments/assets/bd729117-723d-4ea6-9e22-b42105aa5c97" />



<img width="1710" height="881" alt="Screenshot 2025-11-27 at 10 20 43 PM" src="https://github.com/user-attachments/assets/78c8a2d2-c181-4ea8-a46f-794e76fb16c1" />


<img width="1710" height="881" alt="Screenshot 2025-11-27 at 10 21 38 PM" src="https://github.com/user-attachments/assets/2d63c4ff-8f6f-4453-9dab-e27160c5fb82" />


<img width="1710" height="881" alt="Screenshot 2025-11-27 at 10 20 03 PM" src="https://github.com/user-attachments/assets/8801fe45-3621-4628-a29f-b4ddd7086b01" />

