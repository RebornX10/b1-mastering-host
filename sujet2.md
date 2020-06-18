# TP2

## Sujet 2: forensic

### hello world: ez

```c 
#include <stdio.h>

int main(void){
    puts("Hello world");
    return 0;
}
```

![](https://i.imgur.com/vLiw94F.png)

---
### Rarcrack 
######  ouioui il semblerait qu'on est trouvé le meme tuto tous 

1. ouvrir x96dbg.exe en mode x64
2. ouvrir Winrar dans x96bdg : `files`>`open`>`winrar.exe`
3. Aller dans l'onglet `Symbole` ![](https://i.imgur.com/z29MeEi.png)
4. Double Cliker sur ``Winrar.exe`` ![](https://i.imgur.com/wPGzSiI.png)
5. clique droit >`search for`>`current module`>`string reference`
6. chercher "rarkey" ![](https://i.imgur.com/aphhq2Z.png)
7. cliquer sur ``lea r8, qword ptr ds`` ![](https://i.imgur.com/05SJO9O.png)
8. puis remonter la barre de scroll un piti peu pour arriver au 
    ```assembly
    je winrar
    ```
    ![](https://i.imgur.com/jwJDpKm.png)
9. clique droit dessus `binary`>`fill with NOPs`
10. Ne rien changer aux valeurs qui s'affichent et cliquer sur `ok` ![](https://i.imgur.com/SShDKVb.png)
11. Puis Patcher 
12. ![](https://i.imgur.com/J2wRvU9.png) 
    --- plutot simple
    ![](https://i.imgur.com/qUEEhdC.png)
    
12. sauvegarder sous "wincrack.exe"
13. lancer!
14. ![](https://i.imgur.com/KEFBqbJ.png)
    >Le "Evaluation copy" a disparu!
