# Nuestro git-flow

![Gitflow](https://s3-sa-east-1.amazonaws.com/representamecl/cultura/flow-representame.png)

## ¿Cómo lo usamos?

Aquí algunos detalles:

* Para cualquier feat se parte desde staging
* Se debe procurar que staging siempre este actualizado con la última de master
* Para cualquier hotfix se parte desde master
* Una vez que el hotfix se solucione, la rama del hotfix debe ser enviada via un PR a master y staging.
* Todos los branchs (de feat o hotfixes) se deben cerrar una vez que pasen a staging o master respectivamente.