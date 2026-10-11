# 🎣 Entendendo Phishing e Engenharia Social — Desafio DIO (Cibersegurança)

> Projeto educacional desenvolvido como entrega do desafio de laboratório da **DIO (Digital Innovation One)**.
> O objetivo é **compreender como funcionam os ataques de phishing para aprender a se defender deles** — em um ambiente de laboratório isolado, controlado e exclusivamente para fins didáticos.

---

## ⚠️ Aviso Ético e Legal (leia antes de tudo)

Este repositório é **material de estudo defensivo**. Ele documenta, em alto nível, como um ataque de phishing é demonstrado em um laboratório de segurança **para que profissionais e usuários saibam reconhecê-lo e se proteger**.

- ❌ **Não** contém, nem distribui, uma página clonada funcional com captura de credenciais reais.
- ❌ **Não** ensina a atingir pessoas, contas ou sistemas que não sejam seus próprios, em ambiente de teste.
- ✅ Todo o conteúdo pressupõe um **laboratório isolado** (máquinas virtuais em rede fechada) e **contas de teste criadas por você mesmo**.

> **Phishing contra terceiros é crime.** No Brasil, enquadra-se em dispositivos como o Art. 154-A do Código Penal (invasão de dispositivo informático), a Lei nº 12.737/2012 (Lei Carolina Dieckmann), a Lei nº 13.709/2018 (LGPD) e o estelionato (Art. 171). Capturar senhas de terceiros sem autorização expressa e por escrito é ilegal. Use este conhecimento **apenas** de forma ética, legal e autorizada.

---

## 🎯 Objetivos de Aprendizagem

Ao estudar este material, você será capaz de:

- Entender o que é **engenharia social** e por que ela é eficaz;
- Reconhecer a **anatomia de um ataque de phishing** (iscas, páginas falsas, coleta de dados);
- Compreender, em nível conceitual, o papel de ferramentas de demonstração como o **SET (Social-Engineer Toolkit)**;
- Montar um **laboratório seguro e isolado** para estudos de segurança ofensiva/defensiva;
- Aplicar **boas práticas de defesa** contra phishing — a parte mais importante deste desafio.

---

## 🧠 Conceito: o que é Phishing?

**Phishing** é uma técnica de **engenharia social** em que um atacante se passa por uma entidade confiável (um banco, uma rede social, uma empresa) para enganar a vítima e induzi-la a entregar informações sensíveis — tipicamente **usuário e senha**.

O ataque explora o **elo mais frágil da segurança: o ser humano**. Nenhum firewall impede que alguém digite voluntariamente a senha em uma página que *parece* legítima.

### Anatomia de um ataque de phishing

```
┌─────────────┐     ┌──────────────────┐     ┌────────────────────┐
│  1. ISCA    │ --> │ 2. PÁGINA FALSA  │ --> │ 3. COLETA / USO     │
│ (e-mail,    │     │ (clone visual de │     │ (credenciais caem   │
│  SMS, link) │     │  um site real)   │     │  na mão do atacante)│
└─────────────┘     └──────────────────┘     └────────────────────┘
      │                      │                          │
  cria urgência         imita marca,              atacante reutiliza
  e confiança           URL parecida              a senha em outros
                                                  serviços
```

1. **A isca** — Uma mensagem (e-mail, SMS/"smishing", DM) com um pretexto urgente: *"Sua conta será bloqueada", "Você tem uma notificação", "Confirme seus dados"*. Contém um link.
2. **A página falsa** — O link leva a uma página que **imita visualmente** um serviço real (ex.: a tela de login de uma rede social). A URL costuma ser parecida, mas não idêntica (*typosquatting*, subdomínios enganosos, encurtadores).
3. **A coleta** — Ao digitar as credenciais, a vítima as envia para o atacante em vez do serviço real. Como muita gente **reutiliza senhas**, o impacto se espalha para outras contas.

---

## 🧰 Sobre a ferramenta SET (Social-Engineer Toolkit)

O **SET** (`setoolkit`) é um framework **de código aberto** desenvolvido pela TrustedSec, incluído por padrão em distribuições de *pentest* como o **Kali Linux**. Ele é usado por **profissionais de segurança autorizados** para **simular** ataques de engenharia social em testes de intrusão e em **campanhas de conscientização** — por exemplo, para treinar os funcionários de uma empresa a não caírem em golpes.

No contexto deste curso, a ferramenta é apresentada para demonstrar, em laboratório, **como uma página falsa é gerada** — reforçando o entendimento de que:

- clonar a aparência de um site é tecnicamente trivial;
- por isso, **nunca se deve confiar na aparência** de uma página de login;
- a defesa real está em **verificar a URL, usar MFA e gerenciadores de senha**.

> Observação de escopo: este README documenta o **fluxo conceitual** da demonstração do laboratório, como ensinado no curso. Ele **não** fornece uma configuração pronta para expor um clone à internet nem para capturar credenciais de pessoas reais.

### Fluxo conceitual da demonstração (ambiente de laboratório isolado)

Em um laboratório fechado, o módulo de *site cloner* do SET é apresentado assim, em alto nível:

| Etapa | Menu do SET | O que acontece (conceito) |
|-------|-------------|---------------------------|
| 1 | `Social-Engineering Attacks` | Categoria de ataques de engenharia social |
| 2 | `Website Attack Vectors` | Ataques baseados em páginas web |
| 3 | `Credential Harvester Attack Method` | Método de demonstração de coleta de credenciais |
| 4 | `Site Cloner` | Clonagem **visual** de uma página de login |

A ferramenta então hospeda **localmente** (na própria máquina de teste, em rede isolada) uma cópia visual da tela de login. Em um exercício didático, **você acessa sua própria máquina/conta de teste** para observar o comportamento — e, principalmente, para analisar **como identificar que aquela página é falsa**.

---

## 🧪 Montando o laboratório seguro

> **Regra de ouro:** nada sai do laboratório. Rede isolada, contas de teste, zero exposição à internet.

- **Virtualização:** VirtualBox ou VMware.
- **Máquina atacante:** Kali Linux (já traz o SET).
- **Máquina "vítima":** uma segunda VM (ex.: outra instalação Linux/Windows de teste).
- **Rede:** modo **Host-Only / Rede Interna** — as VMs enxergam só umas às outras, **sem acesso à internet**.
- **Contas:** apenas **contas de teste criadas por você**. Nunca use credenciais reais.
- **Snapshots:** tire um *snapshot* antes de começar para restaurar o estado limpo depois.

```
┌───────────────────── Rede Interna / Host-Only (SEM internet) ─────────────────────┐
│                                                                                    │
│     ┌──────────────────┐                        ┌──────────────────────┐           │
│     │  Kali (atacante) │  <───── isolada ─────>  │  VM de teste (vítima)│           │
│     │  SET / setoolkit │                         │  navegador de teste  │           │
│     └──────────────────┘                        └──────────────────────┘           │
│                                                                                    │
└────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🔎 Resultados e observações do laboratório

*(Preencha esta seção com suas próprias anotações e, opcionalmente, capturas de tela na pasta `/images`.)*

- [ ] A cópia visual da página ficou **convincente** à primeira vista?
- [ ] Quais **pistas** denunciavam que a página era falsa? (URL, ausência de cadeado/HTTPS válido, domínio estranho, pequenos erros visuais)
- [ ] O que aconteceu com os dados digitados na **conta de teste**?
- [ ] Como o **MFA** teria frustrado o ataque mesmo que a senha fosse capturada?

> Dica para a entrega: adicione prints do seu laboratório em `images/` e referencie-os aqui, por exemplo:
> `![Tela do menu do SET](images/exemplo-menu-set.png)`

---

## 🛡️ Como se defender de phishing (a parte que mais importa)

Para o usuário final:

- 🔗 **Confira a URL** antes de digitar qualquer senha. Desconfie de domínios estranhos, encurtadores e erros de digitação.
- 🔐 **Ative a autenticação em dois fatores (MFA/2FA).** Mesmo que a senha vaze, o atacante não entra.
- 🧩 **Use um gerenciador de senhas.** Além de senhas únicas por site, ele **não preenche** credenciais em domínios falsos — um ótimo detector de phishing.
- 🚫 **Não clique em links de mensagens urgentes.** Acesse o site digitando o endereço oficial no navegador.
- 📧 **Desconfie do tom de urgência e ameaça** ("sua conta será bloqueada em 24h").

Para organizações:

- 🎓 **Treinamento e simulações autorizadas** de phishing para os colaboradores.
- 🛂 **MFA obrigatório** e políticas de senha fortes.
- 📨 **Filtros anti-phishing**, DMARC/SPF/DKIM e proteção de e-mail.
- 📝 **Processo claro de denúncia** de mensagens suspeitas.

---

## 📂 Estrutura do repositório

```
cibersecurity-desafio-phishing/
├── README.md          # Documentação principal (este arquivo)
├── DISCLAIMER.md      # Termo de uso ético e legal detalhado
├── LICENSE            # Licença (MIT) com aviso educacional
└── images/            # Capturas de tela do seu laboratório (opcional)
```

---

## 📚 Recursos úteis

- Documentação do SET (TrustedSec) — framework open source de engenharia social.
- [GitHub Docs](https://docs.github.com) — guia completo de uso do GitHub.
- [GitHub Markdown](https://docs.github.com/pt/get-started/writing-on-github) — escrevendo com Markdown.
- OWASP — materiais sobre phishing e engenharia social.
- Cartilha de Segurança para Internet (CERT.br / NIC.br).

---

## 👤 Autor

Projeto desenvolvido por **brenoluizdev** como entrega do desafio de laboratório da DIO.

> Material produzido com finalidade **exclusivamente educacional e defensiva**.
