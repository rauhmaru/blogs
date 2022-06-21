# Dicas
![truenas logo](https://www.ixsystems.com/wp-content/uploads/2021/02/truenas_open_storage-logo-full-color-rgb-1280x280.png)

## TrueNAS instalado no pendrive com redundância
Essa não é a melhor forma de se manter um ambiente de produção, porém, para um ambiente de testes funciona muito bem.

### Problema
O TrueNAS em sua instalação não aceita particionamento. Ele utiliza todo o volume. Ou seja, se você possui apenas um volume de 1 TB, ele vai usar para a sua instalação, não deixando nada para ser utilizado como storage.

### Solução: Instalação em um USB Flash (pendrive)
Iremos realizar a instalação em um pendrive, porém, por segurança, iremos manter os dados replicados em um segundo pendrive.
Um modelo que é interessante para realizar esse arranjo (ou armengue?) é o USB Flash Drive Cruzer Fit ou SanDisk Ultra Fit USB 3.1, por conta de seu tamanho.

Nesse procedimento usaremos dois pendrives:
- Um que servirá de instalação primária do TrueNAS;
- E outro que inicialmente será o instalador do TrueNAS, e, após a instalação, ele será utilizado como disco de redundância.

### Configuração
1. Prepare o disco de instalação em um pendrive[^0][^1][^2][^3]
2. Após a instalação via USB (ou qualquer outra), identifique quem é quem: Qual é o que está o SO e qual é que é o da instalação. Uma forma simples de se fazer isso é acessando *Storage > Disks*. Todos os discos conectados serão exibidos. O disco que pertence ao boot-pool é onde está o SO.
3. Aesse *System > Boot*.
4. Na tela *Boot Environments*, clique em *Actions > Boot Pool Status*. Nessa tela, irá aparecer o ponto de montagem do disco (talvez um `/dev/da0p2`).
5. No final da linha, clique nos 3 pontos e selecione *Attach*.
6. Na tela de *Attach*, selecione o novo disco, marque o campo *Use all disk space* e clique *Submit*.
7. O TrueNAS apresentará a mensagem (se nada der errado) *Device Attached*.

Após esses passos, se você voltar a tela *System > Boot > Status*, verá que foi criada a sessão mirror, e abaixo dela estará os dois discos como ONLINE. Com isso, a sua redundância estará completa e terá um pouco mais de segurança.


[^0]: https://www.truenas.com/blog/how-to-install-truenas-core/
[^1]: https://www.truenas.com/docs/core/gettingstarted/install/
[^2]: https://www.truenas.com/community/threads/truenas-on-usb-drive.91273/
[^3]: https://www.youtube.com/watch?v=Wya16ef1G-E
[^4]: https://youtu.be/xTUsJNvLHS8
