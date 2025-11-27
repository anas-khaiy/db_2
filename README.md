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





