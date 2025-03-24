# EasyUI

EasyUI est une bibliothèque Android simple et puissante qui permet d'intégrer facilement des menus, boîtes de dialogue et menus contextuels dans vos applications. Cette bibliothèque réduit considérablement le code nécessaire pour créer des interfaces utilisateur interactives tout en offrant une grande flexibilité de personnalisation.

## Fonctionnalités

- **Boîtes de dialogue** : Créez facilement des boîtes de dialogue d'alerte et des boîtes de dialogue personnalisées
- **Menus** : Implémentez des menus déroulants et contextuels avec un minimum de code
- **Style Material Design** : Tous les composants suivent les directives Material Design
- **API fluide** : Interface de programmation par chaînage (Builder pattern)
- **Personnalisation** : Adaptez facilement l'apparence des composants

## Installation

### Gradle

Ajoutez le dépôt Jitpack à votre fichier `build.gradle` au niveau du projet :

```gradle
allprojects {
    repositories {
        ...
        maven { url 'https://jitpack.io' }
    }
}
```

Puis ajoutez la dépendance dans votre module app :

```gradle
dependencies {
    implementation 'com.github.touzalab:EasyUI:1.0.0'
}
```

### Maven

```xml
<repositories>
    <repository>
        <id>jitpack.io</id>
        <url>https://jitpack.io</url>
    </repository>
</repositories>

<dependency>
    <groupId>com.github.touzalab</groupId>
    <artifactId>EasyUI</artifactId>
    <version>1.0.0</version>
</dependency>
```

## Guide d'utilisation

### Boîtes de dialogue

#### Boîte de dialogue d'alerte

```kotlin
import com.touzalab.EasyUI

// Méthode 1 : Builder pattern
EasyUI.alertDialog(context)
    .setTitle("Information")
    .setMessage("Voici un exemple de boîte de dialogue.")
    .setPositiveButton("OK") {
        // Action lorsque OK est cliqué
    }
    .setNegativeButton("Annuler") {
        // Action lorsque Annuler est cliqué
    }
    .setCancelable(true)
    .show()

// Méthode 2 : Création directe
val dialog = EasyUI.createAlertDialog(context)
dialog.setTitle("Information")
dialog.setMessage("Voici un exemple de boîte de dialogue.")
dialog.setPositiveButton("OK") { 
    // Action lorsque OK est cliqué
}
dialog.setNegativeButton("Annuler") {
    // Action lorsque Annuler est cliqué
}
dialog.show()
```

#### Boîte de dialogue personnalisée

```kotlin
// Création d'une boîte de dialogue avec un layout personnalisé
val dialog = EasyUI.createCustomDialog(context)
dialog.setTitle("Formulaire")
dialog.setView(R.layout.dialog_form) // Votre layout personnalisé
dialog.setPositiveButton("Enregistrer") {
    // Accéder aux vues du layout personnalisé
    val editText = dialog.findViewById<TextInputEditText>(R.id.etName)
    val name = editText?.text.toString()
    // Traitement des données
}
dialog.setNegativeButton("Annuler", null)
dialog.show()
```

### Menus

#### Menu déroulant

```kotlin
// Création d'un menu déroulant attaché à une vue
EasyUI.createMenu(context, anchorView)
    .addItem(1, "Éditer", R.drawable.ic_edit) { id ->
        // Action lorsque "Éditer" est cliqué
    }
    .addItem(2, "Partager", R.drawable.ic_share) { id ->
        // Action lorsque "Partager" est cliqué
    }
    .addItem(3, "Supprimer", R.drawable.ic_delete) { id ->
        // Action lorsque "Supprimer" est cliqué
    }
    .show()
```

#### Menu contextuel

```kotlin
// Création d'un menu contextuel
val contextMenu = EasyUI.createContextMenu(context)
contextMenu.addItem(1, "Copier", R.drawable.ic_copy) { id ->
    // Action lorsque "Copier" est cliqué
}
contextMenu.addItem(2, "Couper", R.drawable.ic_cut) { id ->
    // Action lorsque "Couper" est cliqué
}
contextMenu.addItem(3, "Coller", R.drawable.ic_paste) { id ->
    // Action lorsque "Coller" est cliqué
}

// Afficher le menu contextuel lors d'un appui long
myView.setOnLongClickListener { view ->
    contextMenu.show(view)
    true
}
```

#### Menu déroulant avec TextInputLayout

```kotlin
// Configuration d'un menu déroulant avec TextInputLayout
// XML Layout :
// <com.google.android.material.textfield.TextInputLayout
//     android:id="@+id/tilDropdown"
//     style="@style/Widget.MaterialComponents.TextInputLayout.OutlinedBox.ExposedDropdownMenu"
//     android:layout_width="match_parent"
//     android:layout_height="wrap_content"
//     android:hint="Sélectionnez une option">
//
//     <AutoCompleteTextView
//         android:layout_width="match_parent"
//         android:layout_height="wrap_content"
//         android:inputType="none"/>
//
// </com.google.android.material.textfield.TextInputLayout>

val tilDropdown = findViewById<TextInputLayout>(R.id.tilDropdown)
val dropdownMenu: EasyDropdownMenu<String> = EasyUI.createDropdownMenu(context, tilDropdown)

val items = listOf("Option 1", "Option 2", "Option 3", "Option 4")
dropdownMenu.setItems(items)
dropdownMenu.setOnItemSelectedListener { item ->
    // Action lorsqu'un élément est sélectionné
}
```

## Personnalisation

### Styles et thèmes

Vous pouvez personnaliser l'apparence des composants en modifiant les styles dans votre fichier `themes.xml` :

```xml
<!-- Personnalisation des boîtes de dialogue -->
<style name="AppTheme.Dialog" parent="Theme.MaterialComponents.Light.Dialog">
    <item name="colorPrimary">@color/your_primary_color</item>
    <item name="colorAccent">@color/your_accent_color</item>
</style>

<!-- Application du style personnalisé -->
EasyUI.createAlertDialog(context, R.style.AppTheme.Dialog)
```

### Attributs personnalisables

La bibliothèque offre plusieurs attributs personnalisables pour adapter l'apparence des composants :

```xml
<!-- attrs.xml -->
<declare-styleable name="EasyDialogTheme">
    <attr name="dialogCornerRadius" format="dimension" />
    <attr name="dialogBackgroundColor" format="color" />
    <attr name="dialogTitleColor" format="color" />
    <attr name="dialogMessageColor" format="color" />
    <attr name="dialogButtonTextColor" format="color" />
</declare-styleable>
```

## Exemple complet

Voici un exemple complet d'utilisation de différents composants dans une activité :

```kotlin
import android.os.Bundle
import android.widget.Button
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity
import com.google.android.material.textfield.TextInputLayout
import com.touzalab.EasyUI

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        
        // Exemple 1: Boîte de dialogue d'alerte
        val btnShowAlertDialog = findViewById<Button>(R.id.btnShowAlertDialog)
        btnShowAlertDialog.setOnClickListener {
            EasyUI.alertDialog(this)
                .setTitle("Information")
                .setMessage("Voici un exemple de boîte de dialogue d'alerte.")
                .setPositiveButton("OK") {
                    Toast.makeText(this, "Bouton OK cliqué", Toast.LENGTH_SHORT).show()
                }
                .setNegativeButton("Annuler") {
                    Toast.makeText(this, "Bouton Annuler cliqué", Toast.LENGTH_SHORT).show()
                }
                .setCancelable(true)
                .show()
        }
        
        // Exemple 2: Boîte de dialogue personnalisée
        val btnShowCustomDialog = findViewById<Button>(R.id.btnShowCustomDialog)
        btnShowCustomDialog.setOnClickListener {
            val customDialog = EasyUI.createCustomDialog(this)
            customDialog.setTitle("Formulaire")
            customDialog.setView(R.layout.dialog_form)
            customDialog.setPositiveButton("Enregistrer") {
                val editText = customDialog.findViewById<com.google.android.material.textfield.TextInputEditText>(R.id.etName)
                val name = editText?.text.toString()
                Toast.makeText(this, "Nom enregistré: $name", Toast.LENGTH_SHORT).show()
            }
            customDialog.setNegativeButton("Annuler", null)
            customDialog.show()
        }
        
        // Exemple 3: Menu déroulant
        val btnShowMenu = findViewById<Button>(R.id.btnShowMenu)
        btnShowMenu.setOnClickListener { view ->
            EasyUI.createMenu(this, view)
                .addItem(1, "Éditer", android.R.drawable.ic_menu_edit) { id ->
                    Toast.makeText(this, "Éditer sélectionné", Toast.LENGTH_SHORT).show()
                }
                .addItem(2, "Partager", android.R.drawable.ic_menu_share) { id ->
                    Toast.makeText(this, "Partager sélectionné", Toast.LENGTH_SHORT).show()
                }
                .addItem(3, "Supprimer", android.R.drawable.ic_menu_delete) { id ->
                    Toast.makeText(this, "Supprimer sélectionné", Toast.LENGTH_SHORT).show()
                }
                .show()
        }
        
        // Exemple 4: Menu contextuel sur appui long
        val tvContextMenu = findViewById<View>(R.id.tvContextMenu)
        tvContextMenu.setOnLongClickListener { view ->
            EasyUI.createContextMenu(this)
                .addItem(1, "Copier", android.R.drawable.ic_menu_edit) { id ->
                    Toast.makeText(this, "Copier sélectionné", Toast.LENGTH_SHORT).show()
                }
                .addItem(2, "Couper", android.R.drawable.ic_menu_share) { id ->
                    Toast.makeText(this, "Couper sélectionné", Toast.LENGTH_SHORT).show()
                }
                .addItem(3, "Coller", android.R.drawable.ic_menu_delete) { id ->
                    Toast.makeText(this, "Coller sélectionné", Toast.LENGTH_SHORT).show()
                }
                .show(view)
            true
        }
        
        // Exemple 5: Menu déroulant avec TextInputLayout
        val tilDropdown = findViewById<TextInputLayout>(R.id.tilDropdown)
        val dropdownMenu: EasyDropdownMenu<String> = EasyUI.createDropdownMenu(this, tilDropdown)
        
        val items = listOf("Option 1", "Option 2", "Option 3", "Option 4")
        dropdownMenu.setItems(items)
        dropdownMenu.setOnItemSelectedListener { item ->
            Toast.makeText(this, "Sélectionné: $item", Toast.LENGTH_SHORT).show()
        }
    }
}
```

## Classes principales

### Boîtes de dialogue
- `EasyDialog` : Classe de base pour toutes les boîtes de dialogue
- `EasyAlertDialog` : Boîte de dialogue avec titre, message et boutons
- `EasyCustomDialog` : Boîte de dialogue avec contenu personnalisé

### Menus
- `EasyMenu` : Menu déroulant standard
- `EasyContextMenu` : Menu contextuel personnalisé
- `EasyDropdownMenu` : Menu déroulant basé sur TextInputLayout et AutoCompleteTextView

### Point d'entrée
- `EasyUI` : Objet principal pour créer tous les types de composants

## Compatibilité

- Compatible Android API 21 (Lollipop) et supérieur
- Intégration avec les composants Material Design
- Compatible avec AndroidX

## Licence

```
MIT License

Copyright (c) 2025 TouzaLab

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

## Contributions

Les contributions sont les bienvenues ! N'hésitez pas à ouvrir une issue ou soumettre une pull request sur le dépôt GitHub.

## Contact

Pour toute question ou assistance, contactez-nous à support@2zalab.com ou ouvrez une issue sur GitHub.
