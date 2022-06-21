# Dicas
![truenas logo](https://www.ixsystems.com/wp-content/uploads/2021/02/truenas_open_storage-logo-full-color-rgb-1280x280.png)

<details><summary>TrueNAS instalado no pendrive com redundância</summary>
<p>
  
Essa não é a melhor forma de se manter um ambiente de produção, porém, para um ambiente de testes funciona muito bem. O ideal é que se utilize dois discos pequenos, talvez SSD, mas... Nem sempre é possível.

### Problema
O TrueNAS em sua instalação não aceita particionamento. Ele utiliza todo o volume. Ou seja, se você possui apenas um volume de 1 TB, ele vai usar para a sua instalação, não deixando nada para ser utilizado como storage.

**Acha que não é um problema?** Bom, imagine que você possui uma máquina com 6 discos, todos SAS de 600 GB, 15k rpm. Daí, você quer usar o TrueNAS. Bom, como ele precisa de um volume inteiro, você vai ter de separar dois discos de 600 GB para criar um RAID 1, e instalar o TrueNAS nele. Bem... Você deu 600 GB para uma instalação que... Não usou nem 10 GB. Desperdício.

Dói né?

### Solução: Instalação em um USB Flash (pendrive)
A instalação em um USB flash é uma solução de baixo custo, para poder utilizar toda a capacidade dos discos HDD ou SSD instalados na máquina. Por segurança, iremos manter os dados replicados em um segundo pendrive, criando um mirror (algo como um RAID 1).

Um modelo que é interessante para realizar esse arranjo (ou solução de contorno?) é o USB Flash Drive Cruzer Fit ou SanDisk Ultra Fit USB 3.1, por conta de seu tamanho.

<img src="https://www.westerndigital.com/content/dam/store/en-us/assets/products/usb-flash-drives/ultra-fit-usb-3-1/gallery/ultra-fit-usb-3-1-angle-right-up.png.wdthumb.1280.1280.webp" alt="USB Flash Drive Cruzer Fit" width="400"/>

Nesse procedimento usaremos dois pendrives:
- Um que servirá de disco para a instalação do TrueNAS;
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
  
  
  Ainda com dúvidas?
  O procedimento inteiro pode ser visto aqui: https://www.youtube.com/watch?v=xTUsJNvLHS8
</p>
</details>

[^0]: https://www.truenas.com/blog/how-to-install-truenas-core/
[^1]: https://www.truenas.com/docs/core/gettingstarted/install/
[^2]: https://www.truenas.com/community/threads/truenas-on-usb-drive.91273/
[^3]: https://www.youtube.com/watch?v=Wya16ef1G-E
[^4]: https://youtu.be/xTUsJNvLHS8
