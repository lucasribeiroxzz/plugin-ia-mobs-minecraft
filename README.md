<p align="center">
  <img src="https://img.shields.io/badge/Minecraft-1.20+-green?style=for-the-badge&logo=minecraft" alt="Minecraft 1.20+"/>
  <img src="https://img.shields.io/badge/Paper%20%2F%20Spigot-API-blue?style=for-the-badge" alt="Paper/Spigot"/>
  <img src="https://img.shields.io/badge/Java-17+-orange?style=for-the-badge&logo=openjdk" alt="Java 17+"/>
  <img src="https://img.shields.io/badge/Versão-1.0.0-purple?style=for-the-badge" alt="Version"/>
  <img src="https://img.shields.io/badge/Licença-MIT-yellow?style=for-the-badge" alt="License"/>
</p>

<h1 align="center">🧠 MobAI — Inteligência Artificial Avançada para Mobs</h1>
<h3 align="center">🧠 MobAI — Advanced Mob Artificial Intelligence</h3>

<p align="center">
  <b>🇧🇷 Transforme os mobs do seu servidor em inimigos inteligentes, estratégicos e desafiadores.</b><br/>
  <b>🇺🇸 Transform your server's mobs into intelligent, strategic, and challenging enemies.</b><br/><br/>
  Comportamento em grupo • IA Adaptativa • Bosses com Fases • Dificuldade Dinâmica<br/>
  Group Behavior • Adaptive AI • Multi-Phase Bosses • Dynamic Difficulty
</p>

<p align="center">
  Desenvolvido por <b>Lucas Ribeiro</b> — <b>BLMP UNIVERSE</b> 🌌
</p>

---

## 📋 Índice / Table of Contents

<table>
<tr><td>

**🇧🇷 Português**
- [Sobre](#-sobre)
- [Funcionalidades](#-funcionalidades)
- [Como a IA Funciona](#-como-a-ia-funciona)
- [Bosses Customizados](#-bosses-customizados)
- [Comandos](#-comandos)
- [Permissões](#-permissões)
- [Sistema de Dificuldade](#-sistema-de-dificuldade)
- [Idiomas](#-idiomas)
- [Instalação](#-instalação)
- [Compilação](#-compilação)
- [Configuração](#%EF%B8%8F-configuração)
- [Extensibilidade](#-adicionando-novos-comportamentos)
- [Estrutura do Projeto](#-estrutura-do-projeto)

</td><td>

**🇺🇸 English**
- [About](#-about)
- [Features](#-features)
- [How the AI Works](#-how-the-ai-works)
- [Custom Bosses](#-custom-bosses)
- [Commands](#-commands-1)
- [Permissions](#-permissions)
- [Difficulty System](#-difficulty-system)
- [Languages](#-languages)
- [Installation](#-installation)
- [Building](#-building)
- [Configuration](#%EF%B8%8F-configuration)
- [Extensibility](#-adding-new-behaviors)
- [Project Structure](#-project-structure)

</td></tr>
</table>

---

<h1 align="center">🇧🇷 Português</h1>

---

## 📖 Sobre

**MobAI** é um plugin avançado para servidores **Paper/Spigot 1.20+** que substitui a IA burra dos mobs do Minecraft por um sistema inteligente de tomada de decisão.

Mobs agora recuam quando estão com pouca vida, esqueletos mantêm distância e se esquivam, zumbis cercam o jogador em grupo, e bosses customizados possuem múltiplas fases com habilidades devastadoras.

| Vanilla Minecraft | Com MobAI |
|---|---|
| Mobs andam em linha reta até o jogador | Mobs cercam, recuam e se posicionam estrategicamente |
| Sem coordenação entre mobs | Mobs alertam uns aos outros e atacam em grupo |
| Mobs não lembram de nada | Sistema de memória com mapa de ameaças |
| Dificuldade fixa | 4 níveis de dificuldade dinâmica |
| Sem bosses customizados | 2 bosses épicos com múltiplas fases |

---

## ✨ Funcionalidades

### 🐺 Comportamento em Grupo
- **Alvo compartilhado** — quando um mob detecta um jogador, alerta os mobs próximos
- **Cerco por zumbis** — zumbis se distribuem em ângulos para cercar o jogador
- **Posicionamento de esqueletos** — mantêm distância ideal, recuam e fazem strafing
- **Ataque coordenado** — contra jogadores com armadura forte, mobs se coordenam

### 🧠 IA Adaptativa
- **Fuga** — mobs com vida crítica (<15%) fogem na direção oposta
- **Recuo tático** — mobs com vida baixa (<25%) recuam e fazem side-stepping
- **Buscar cobertura** — mobs feridos buscam obstáculos para se proteger
- **Consciência ambiental** — de noite ficam mais rápidos, em tempestades ganham força
- **Memória curta** — mobs lembram quem os atacou e priorizam alvos perigosos

### 🏗️ Interação com Ambiente
- **Quebrar blocos** — zumbis podem quebrar portas, vidros e portões (animação progressiva)
- **Usar cobertura** — mobs buscam blocos sólidos para se esconder
- **Pathfinding customizado** — movimentos estratégicos via Paper API

### ⚔️ Sistema de Bosses
- **2 bosses customizados** com múltiplas fases e **9 habilidades** únicas
- **Boss bar** dinâmica com mudança de cor por fase
- **Cooldown** de habilidades afetado pela dificuldade
- **Loot** e **anúncio global** ao spawnar/derrotar

### 📊 Performance
- **Round-robin** — processa N mobs por tick (configurável)
- **Agrupamento eficiente** por proximidade
- **Sem loops pesados** — tasks assíncronas do Bukkit

---

## 🧠 Como a IA Funciona

Sistema de **Seletor de Comportamento por Prioridade**:

```
┌─────────────────────────────────────────────────────────┐
│                    AI Tick (cada 5 ticks)                │
├─────────────────────────────────────────────────────────┤
│  Para cada mob rastreado:                               │
│                                                         │
│  ① Buscar melhor alvo (grupo → memória → vanilla)      │
│  ② Avaliar comportamentos por prioridade:               │
│                                                         │
│     Prioridade 1:  FleeIfLowHealth     (vida < 15%)    │
│     Prioridade 5:  RetreatIfDamaged    (vida < 25%)    │
│     Prioridade 7:  SeekCover           (buscar abrigo)  │
│     Prioridade 8:  SkeletonPosition    (manter dist.)   │
│     Prioridade 9:  GroupAttack         (alvo forte)     │
│     Prioridade 10: ZombieSurround      (cercar alvo)    │
│     Prioridade 12: ZombieBreakBlocks   (quebrar portas) │
│     Prioridade 15: AlertNearby         (alertar grupo)  │
│     Prioridade 20: EnvironmentAware    (noite/chuva)    │
│                                                         │
│  ③ Primeiro que retorna TRUE executa                    │
│  ④ Debug: partículas + action bar                       │
└─────────────────────────────────────────────────────────┘
```

---

## 👹 Bosses Customizados

### ☠️ Rei dos Mortos (Undead King)

| Fase | Vida | Habilidades |
|------|------|-------------|
| **Fase 1** | 100% → 70% | Melee + **Invocar Lacaios** (3 zumbis) |
| **Fase 2** | 70% → 30% | + **Golpe no Chão** (AoE + knockback), escuridão, +armadura |
| **Fase 3** | 30% → 0% | + **Investida** + **Roubar Vida**, berserk, 6 lacaios |

> 300 HP base • Netherite • Resistente a fogo • Loot: Nether Star, Diamantes, XP

### 🔥 Dragão Infernal (Inferno Dragon)

| Fase | Vida | Habilidades |
|------|------|-------------|
| **Fase 1** | 100% → 70% | **Rajada de Fogo** (3 tiros) + **Anel de Fogo** |
| **Fase 2** | 70% → 30% | + **Chuva de Meteoros** (5 fireballs) + **Invocar Blazes** |
| **Fase 3** | 30% → 0% | + **Tornado de Fogo**, aura constante, 8 meteoros |

> 400 HP base • Voa • Aura de fogo fase 3 • Loot: Nether Star, Blaze Rods, Diamantes, XP

---

## 💬 Comandos

| Comando | Descrição |
|---------|-----------|
| `/mobai` | Menu de ajuda |
| `/mobai difficulty <nível>` | Alterar dificuldade (`EASY`/`NORMAL`/`HARD`/`INSANE`) |
| `/mobai debug` | Ativar/desativar modo debug |
| `/mobai reload` | Recarregar configuração |
| `/mobai boss spawn <tipo>` | Spawnar boss (`undead_king`/`inferno_dragon`) |
| `/mobai boss list` | Listar bosses disponíveis e ativos |
| `/mobai info` | Informações do plugin |
| `/mobai stats` | Estatísticas da IA |
| `/mobai lang <pt\|en>` | Mudar idioma |

> **Alias:** `/mai`

---

## 🔑 Permissões

| Permissão | Descrição | Padrão |
|-----------|-----------|--------|
| `mobai.admin` | Acesso total | OP |
| `mobai.debug` | Modo debug | OP |
| `mobai.difficulty` | Alterar dificuldade | OP |

---

## 📈 Sistema de Dificuldade

| | EASY | NORMAL | HARD | INSANE |
|---|:---:|:---:|:---:|:---:|
| **Dano** | 0.75x | 1.0x | 1.5x | **2.0x** |
| **Vida** | 0.8x | 1.0x | 1.5x | **2.0x** |
| **Cooldowns** | 1.5x | 1.0x | 0.7x | **0.4x** |
| **Coordenação** | 0.5x | 1.0x | 1.5x | **2.0x** |

---

## 🌍 Idiomas

```
/mobai lang pt    → Português 🇧🇷
/mobai lang en    → English 🇺🇸
```

Arquivos editáveis em `plugins/MobAI/messages_pt.yml` e `messages_en.yml`.

---

## 📦 Instalação

1. Baixe `MobAI-1.0.0.jar` na aba [Releases](../../releases)
2. Coloque na pasta `plugins/` do servidor
3. Reinicie o servidor
4. Configure em `plugins/MobAI/config.yml`
5. `/mobai difficulty HARD` para começar!

**Requisitos:** Paper/Spigot **1.20+** • Java **17+**

---

## 🔨 Compilação

```bash
git clone https://github.com/seu-usuario/MobAI.git
cd MobAI
mvn clean package -DskipTests
# JAR em: target/MobAI-1.0.0.jar
```

---

## ⚙️ Configuração

<details>
<summary>📄 Ver config.yml</summary>

```yaml
difficulty: NORMAL
language: pt
debug: false

performance:
  max-mobs-per-tick: 20
  ai-update-interval: 5
  group-range: 32.0
  max-group-size: 8
  memory-duration: 600

group-behavior:
  shared-targeting: true
  skeleton-positioning: true
  zombie-surround: true
  alert-range: 24.0

adaptive-ai:
  retreat-health-percent: 25.0
  flee-health-percent: 15.0
  player-strength-threshold: 15
  environment-awareness: true
  short-memory: true

environment:
  zombie-block-breaking: true
  use-cover: true

bosses:
  enabled: true
  announce-spawn: true
  health-bar-color: RED
```

</details>

---

## 🧩 Adicionando Novos Comportamentos

```java
public class CreeperStealth implements MobBehavior {
    @Override public String getName() { return "CreeperStealth"; }
    @Override public int getPriority() { return 8; }
    
    @Override
    public boolean shouldExecute(LivingEntity mob, Player target) {
        return target != null && !isPlayerLooking(mob, target);
    }
    
    @Override
    public void execute(LivingEntity mob, Player target) {
        if (mob instanceof Mob m)
            m.getPathfinder().moveTo(target.getLocation(), 0.8);
    }
}

// Registrar em MobAIManager.registerDefaultBehaviors():
BehaviorRegistry.register(EntityType.CREEPER, new CreeperStealth());
```

---

---

<h1 align="center">🇺🇸 English</h1>

---

## 📖 About

**MobAI** is an advanced plugin for **Paper/Spigot 1.20+** servers that replaces Minecraft's dumb mob AI with an intelligent decision-making system.

Mobs now retreat when low on health, skeletons maintain distance and strafe, zombies surround the player in groups, and custom bosses have multiple phases with devastating abilities.

| Vanilla Minecraft | With MobAI |
|---|---|
| Mobs walk in a straight line to the player | Mobs surround, retreat, and position strategically |
| No coordination between mobs | Mobs alert each other and attack as a group |
| Mobs don't remember anything | Memory system with threat maps |
| Fixed difficulty | 4 dynamic difficulty levels |
| No custom bosses | 2 epic bosses with multiple phases |

---

## ✨ Features

### 🐺 Group Behavior
- **Shared targeting** — when one mob detects a player, nearby mobs are alerted
- **Zombie surrounding** — zombies spread at different angles to surround the player
- **Skeleton positioning** — maintain ideal distance, retreat and strafe while shooting
- **Coordinated attack** — against well-armored players, mobs coordinate together

### 🧠 Adaptive AI
- **Flee** — mobs with critical health (<15%) run in the opposite direction
- **Tactical retreat** — mobs with low health (<25%) back off and side-step
- **Seek cover** — damaged mobs look for solid obstacles for protection
- **Environmental awareness** — faster at night, stronger during storms
- **Short memory** — mobs remember who attacked them and prioritize dangerous targets

### 🏗️ Environment Interaction
- **Block breaking** — zombies can break doors, glass, and fence gates (progressive animation)
- **Use cover** — mobs seek solid blocks for protection
- **Custom pathfinding** — strategic movement via Paper API

### ⚔️ Boss System
- **2 custom bosses** with multiple phases and **9 unique abilities**
- **Dynamic boss bar** with color change per phase
- **Ability cooldowns** affected by difficulty
- **Loot drops** and **global announcements**

### 📊 Performance
- **Round-robin** processing — configurable mobs per tick
- **Efficient grouping** by proximity
- **No heavy loops** — Bukkit async tasks

---

## 🧠 How the AI Works

**Priority-Based Behavior Selector** system:

```
┌─────────────────────────────────────────────────────────┐
│                    AI Tick (every 5 ticks)               │
├─────────────────────────────────────────────────────────┤
│  For each tracked mob:                                  │
│                                                         │
│  ① Find best target (group → memory → vanilla)         │
│  ② Evaluate behaviors by priority:                      │
│                                                         │
│     Priority 1:  FleeIfLowHealth     (HP < 15%)        │
│     Priority 5:  RetreatIfDamaged    (HP < 25%)        │
│     Priority 7:  SeekCover           (find shelter)     │
│     Priority 8:  SkeletonPosition    (keep distance)    │
│     Priority 9:  GroupAttack         (strong target)    │
│     Priority 10: ZombieSurround      (surround target)  │
│     Priority 12: ZombieBreakBlocks   (break doors)      │
│     Priority 15: AlertNearby         (alert group)      │
│     Priority 20: EnvironmentAware    (night/rain)       │
│                                                         │
│  ③ First behavior returning TRUE executes               │
│  ④ Debug: particles + action bar                        │
└─────────────────────────────────────────────────────────┘
```

---

## 👹 Custom Bosses

### ☠️ Undead King

| Phase | Health | Abilities |
|-------|--------|-----------|
| **Phase 1** | 100% → 70% | Melee + **Summon Minions** (3 zombies) |
| **Phase 2** | 70% → 30% | + **Ground Slam** (AoE + knockback), darkness, +armor |
| **Phase 3** | 30% → 0% | + **Dash Attack** + **Lifesteal**, berserk, 6 minions |

> 300 HP base • Netherite gear • Fire resistant • Loot: Nether Star, Diamonds, XP

### 🔥 Inferno Dragon

| Phase | Health | Abilities |
|-------|--------|-----------|
| **Phase 1** | 100% → 70% | **Fireball Barrage** (3 shots) + **Fire Ring** |
| **Phase 2** | 70% → 30% | + **Meteor Shower** (5 fireballs) + **Summon Blazes** |
| **Phase 3** | 30% → 0% | + **Fire Tornado**, constant fire aura, 8 meteors |

> 400 HP base • Flies • Fire aura phase 3 • Loot: Nether Star, Blaze Rods, Diamonds, XP

---

## 💬 Commands

| Command | Description |
|---------|-------------|
| `/mobai` | Help menu |
| `/mobai difficulty <level>` | Set difficulty (`EASY`/`NORMAL`/`HARD`/`INSANE`) |
| `/mobai debug` | Toggle debug mode |
| `/mobai reload` | Reload configuration |
| `/mobai boss spawn <type>` | Spawn boss (`undead_king`/`inferno_dragon`) |
| `/mobai boss list` | List available and active bosses |
| `/mobai info` | Plugin info |
| `/mobai stats` | AI statistics |
| `/mobai lang <pt\|en>` | Change language |

> **Alias:** `/mai`

---

## 🔑 Permissions

| Permission | Description | Default |
|-----------|-------------|---------|
| `mobai.admin` | Full access | OP |
| `mobai.debug` | Debug mode | OP |
| `mobai.difficulty` | Change difficulty | OP |

---

## 📈 Difficulty System

| | EASY | NORMAL | HARD | INSANE |
|---|:---:|:---:|:---:|:---:|
| **Damage** | 0.75x | 1.0x | 1.5x | **2.0x** |
| **Health** | 0.8x | 1.0x | 1.5x | **2.0x** |
| **Cooldowns** | 1.5x | 1.0x | 0.7x | **0.4x** |
| **Coordination** | 0.5x | 1.0x | 1.5x | **2.0x** |

---

## 🌍 Languages

```
/mobai lang pt    → Português 🇧🇷
/mobai lang en    → English 🇺🇸
```

Editable files in `plugins/MobAI/messages_pt.yml` and `messages_en.yml`.

---

## 📦 Installation

1. Download `MobAI-1.0.0.jar` from [Releases](../../releases)
2. Place in your server's `plugins/` folder
3. Restart the server
4. Configure in `plugins/MobAI/config.yml`
5. `/mobai difficulty HARD` to start!

**Requirements:** Paper/Spigot **1.20+** • Java **17+**

---

## 🔨 Building

```bash
git clone https://github.com/your-user/MobAI.git
cd MobAI
mvn clean package -DskipTests
# JAR at: target/MobAI-1.0.0.jar
```

---

## ⚙️ Configuration

<details>
<summary>📄 View config.yml</summary>

```yaml
difficulty: NORMAL
language: pt
debug: false

performance:
  max-mobs-per-tick: 20
  ai-update-interval: 5
  group-range: 32.0
  max-group-size: 8
  memory-duration: 600

group-behavior:
  shared-targeting: true
  skeleton-positioning: true
  zombie-surround: true
  alert-range: 24.0

adaptive-ai:
  retreat-health-percent: 25.0
  flee-health-percent: 15.0
  player-strength-threshold: 15
  environment-awareness: true
  short-memory: true

environment:
  zombie-block-breaking: true
  use-cover: true

bosses:
  enabled: true
  announce-spawn: true
  health-bar-color: RED
```

</details>

---

## 🧩 Adding New Behaviors

```java
public class CreeperStealth implements MobBehavior {
    @Override public String getName() { return "CreeperStealth"; }
    @Override public int getPriority() { return 8; }
    
    @Override
    public boolean shouldExecute(LivingEntity mob, Player target) {
        return target != null && !isPlayerLooking(mob, target);
    }
    
    @Override
    public void execute(LivingEntity mob, Player target) {
        if (mob instanceof Mob m)
            m.getPathfinder().moveTo(target.getLocation(), 0.8);
    }
}

// Register in MobAIManager.registerDefaultBehaviors():
BehaviorRegistry.register(EntityType.CREEPER, new CreeperStealth());
```

---

## 📂 Estrutura do Projeto / Project Structure

```
src/main/java/com/mobai/
├── MobAIPlugin.java              # Main plugin class
├── config/
│   ├── ConfigManager.java        # config.yml management
│   └── LanguageManager.java      # i18n system (PT/EN)
├── difficulty/
│   ├── DifficultyLevel.java      # EASY/NORMAL/HARD/INSANE enum
│   └── DifficultyManager.java    # Difficulty multipliers
├── ai/
│   ├── MobBehavior.java          # Behavior interface
│   ├── BehaviorRegistry.java     # Type → behaviors mapping
│   ├── MobMemory.java            # Memory system
│   ├── MobAIManager.java         # Main AI loop
│   └── behaviors/
│       ├── FleeIfLowHealth.java          # Priority 1
│       ├── RetreatIfDamaged.java         # Priority 5
│       ├── SeekCover.java                # Priority 7
│       ├── SkeletonStrategicPosition.java# Priority 8
│       ├── GroupAttackStrongPlayer.java  # Priority 9
│       ├── ZombieSurroundTarget.java     # Priority 10
│       ├── ZombieBreakBlocks.java        # Priority 12
│       ├── AlertNearbyMobs.java          # Priority 15
│       └── EnvironmentAwareness.java     # Priority 20
├── boss/
│   ├── BossAbility.java          # Ability interface
│   ├── CustomBoss.java           # Boss interface
│   ├── BossManager.java          # Boss manager
│   └── impl/
│       ├── UndeadKing.java       # Boss: Undead King / Rei dos Mortos
│       └── InfernoDragon.java    # Boss: Inferno Dragon / Dragão Infernal
├── commands/
│   ├── MobAICommand.java         # Command executor
│   └── MobAITabCompleter.java    # Tab completion
└── listeners/
    ├── MobEventListener.java     # Mob events
    └── BossEventListener.java    # Boss events
```

---

## 📝 Licença / License

Este projeto está sob a licença MIT. / This project is licensed under the MIT License.

---

<p align="center">
  <br/>
  <img src="https://img.shields.io/badge/BLMP-UNIVERSE-blueviolet?style=for-the-badge&logoColor=white" alt="BLMP UNIVERSE"/>
  <br/><br/>
  Desenvolvido com ☕ e 🧠 por <b>Lucas Ribeiro</b><br/>
  Developed with ☕ and 🧠 by <b>Lucas Ribeiro</b><br/><br/>
  <b>🌌 BLMP UNIVERSE 🌌</b><br/><br/>
  ⭐ Se este projeto te ajudou, deixe uma estrela! / If this helped you, leave a star! ⭐
</p>
