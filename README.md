Assets/
├── Scripts/
│   ├── PlayerController.cs
│   ├── ThirdPersonCamera.cs
│   ├── Gun.cs
│   ├── Enemy.cs
│   └── Health.cs
├── Prefabs/
│   ├── Player.prefab
│   ├── Enemy.prefab
│   └── Bullet.prefab
├── Scenes/
│   └── MainScene.unity
├── Materials/
├── Models/
└── UI/
    └── HUD.prefab
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    public float moveSpeed = 5f;
    public float rotationSpeed = 720f;
    public Animator animator;
    public CharacterController controller;
    public Transform cameraTransform;

    void Update()
    {
        float h = Input.GetAxis("Horizontal");
        float v = Input.GetAxis("Vertical");
        Vector3 move = new Vector3(h, 0, v);
        move = cameraTransform.forward * v + cameraTransform.right * h;
        move.y = 0;
        controller.Move(move * moveSpeed * Time.deltaTime);

        if (move != Vector3.zero)
        {
            Quaternion targetRotation = Quaternion.LookRotation(move);
            transform.rotation = Quaternion.RotateTowards(transform.rotation, targetRotation, rotationSpeed * Time.deltaTime);
        }

        animator.SetFloat("Speed", move.magnitude);
    }
}
import tkinter as tk
import random
from playsound import playsound
import threading

def tocar_som(caminho):
    threading.Thread(target=playsound, args=(caminho,), daemon=True).start()

def iniciar_jogo():
    global numero_secreto, tentativas, limite_maximo
    dificuldade = dificuldade_var.get()
    
    if dificuldade == "Fácil":
        limite_maximo = 50
        tentativas = 7
    elif dificuldade == "Médio":
        limite_maximo = 100
        tentativas = 5
    elif dificuldade == "Difícil":
        limite_maximo = 200
        tentativas = 4

    numero_secreto = random.randint(1, limite_maximo)
    tentativas_label["text"] = f"Tentativas: {tentativas}"
    entry.config(state="normal")
    botao.config(state="normal")
    resultado["text"] = ""
    entry.delete(0, tk.END)
    titulo["text"] = f"Adivinhe um número de 1 a {limite_maximo}"

def verificar_chute():
    global tentativas, pontuacao
    try:
        chute = int(entry.get())
        tentativas -= 1

        if chute == numero_secreto:
            resultado["text"] = f"Acertou! Era {numero_secreto}!"
            pontuacao += 10
            pontuacao_label["text"] = f"Pontuação: {pontuacao}"
            desativar_jogo()
            tocar_som("acerto.mp3")
        elif chute < numero_secreto:
            resultado["text"] = "Tente um número maior!"
            tocar_som("erro.mp3")
        else:
            resultado["text"] = "Tente um número menor!"
            tocar_som("erro.mp3")

        if tentativas == 0 and chute != numero_secreto:
            resultado["text"] = f"Você perdeu! Era {numero_secreto}."
            desativar_jogo()
            tocar_som("erro.mp3")

        tentativas_label["text"] = f"Tentativas: {tentativas}"
    except ValueError:
        resultado["text"] = "Digite um número válido."

def desativar_jogo():
    entry.config(state="disabled")
    botao.config(state="disabled")

# Variáveis globais
numero_secreto = 0
tentativas = 0
pontuacao = 0
limite_maximo = 100

# Janela
janela = tk.Tk()
janela.title("Jogo da Adivinhação - Versão com Dificuldade e Som")

titulo = tk.Label(janela, text="Escolha a dificuldade", font=("Arial", 14))
titulo.pack()

# Dificuldade
dificuldade_var = tk.StringVar(value="Médio")
tk.OptionMenu(janela, dificuldade_var, "Fácil", "Médio", "Difícil").pack()

tk.Button(janela, text="Começar Jogo", command=iniciar_jogo).pack(pady=5)

entry = tk.Entry(janela, font=("Arial", 12))
entry.pack()

botao = tk.Button(janela, text="Chutar", command=verificar_chute)
botao.pack()

resultado = tk.Label(janela, text="", font=("Arial", 12))
resultado.pack()

tentativas_label = tk.Label(janela, text="Tentativas:")
tentativas_label.pack()

pontuacao_label = tk.Label(janela, text="Pontuação: 0")
pontuacao_label.pack()

janela.mainloop()
import pygame
import sys

# Inicializar o Pygame
pygame.init()

# Tela
largura, altura = 800, 600
tela = pygame.display.set_mode((largura, altura))
pygame.display.set_caption("Protótipo estilo Free Fire")

# Jogador
jogador = pygame.Rect(400, 300, 40, 40)
velocidade = 5

# Cores
PRETO = (0, 0, 0)
AZUL = (0, 100, 255)

# Loop do jogo
while True:
    for evento in pygame.event.get():
        if evento.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    # Teclado
    teclas = pygame.key.get_pressed()
    if teclas[pygame.K_w]:
        jogador.y -= velocidade
    if teclas[pygame.K_s]:
        jogador.y += velocidade
    if teclas[pygame.K_a]:
        jogador.x -= velocidade
    if teclas[pygame.K_d]:
        jogador.x += velocidade

    # Atualizar tela
    tela.fill(PRETO)
    pygame.draw.rect(tela, AZUL, jogador)
    pygame.display.flip()
    pygame.time.Clock().tick(60)
    import pygame
import sys
import math

# Inicializar Pygame
pygame.init()

# Tela
largura, altura = 800, 600
tela = pygame.display.set_mode((largura, altura))
pygame.display.set_caption("Protótipo Estilo Free Fire")

# Jogador
jogador = pygame.Rect(400, 300, 40, 40)
velocidade = 5

# Projéteis (lista)
projeteis = []
vel_tiro = 10

# Cores
PRETO = (0, 0, 0)
AZUL = (0, 100, 255)
VERMELHO = (255, 50, 50)

# Loop principal
clock = pygame.time.Clock()
while True:
    for evento in pygame.event.get():
        if evento.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

        # Clique do mouse para atirar
        if evento.type == pygame.MOUSEBUTTONDOWN and evento.button == 1:
            mouse_x, mouse_y = pygame.mouse.get_pos()
            dx = mouse_x - jogador.centerx
            dy = mouse_y - jogador.centery
            angulo = math.atan2(dy, dx)
            projeteis.append({
                "x": jogador.centerx,
                "y": jogador.centery,
                "dx": math.cos(angulo) * vel_tiro,
                "dy": math.sin(angulo) * vel_tiro
            })

    # Movimento do jogador
    teclas = pygame.key.get_pressed()
    if teclas[pygame.K_w]:
        jogador.y -= velocidade
    if teclas[pygame.K_s]:
        jogador.y += velocidade
    if teclas[pygame.K_a]:
        jogador.x -= velocidade
    if teclas[pygame.K_d]:
        jogador.x += velocidade

    # Atualizar posição dos projéteis
    for p in projeteis:
        p["x"] += p["dx"]
        p["y"] += p["dy"]
    projeteis = [p for p in projeteis if 0 < p["x"] < largura and 0 < p["y"] < altura]

    # Desenho
    tela.fill(PRETO)
    pygame.draw.rect(tela, AZUL, jogador)
    for p in projeteis:
        pygame.draw.circle(tela, VERMELHO, (int(p["x"]), int(p["y"])), 5)
    pygame.display.flip()
    clock.tick(60)
    import tkinter as tk
import random
from playsound import playsound
import threading

def tocar_som(caminho):
    threading.Thread(target=playsound, args=(caminho,), daemon=True).start()

def iniciar_jogo():
    global numero_secreto, tentativas, limite_maximo
    dificuldade = dificuldade_var.get()
    
    if dificuldade == "Fácil":
        limite_maximo = 50
        tentativas = 7
    elif dificuldade == "Médio":
        limite_maximo = 100
        tentativas = 5
    elif dificuldade == "Difícil":
        limite_maximo = 200
        tentativas = 4

    numero_secreto = random.randint(1, limite_maximo)
    tentativas_label["text"] = f"Tentativas: {tentativas}"
    entry.config(state="normal")
    botao.config(state="normal")
    resultado["text"] = ""
    entry.delete(0, tk.END)
    titulo["text"] = f"Adivinhe um número de 1 a {limite_maximo}"

def verificar_chute():
    global tentativas, pontuacao
    try:
        chute = int(entry.get())
        tentativas -= 1

        if chute == numero_secreto:
            resultado["text"] = f"Acertou! Era {numero_secreto}!"
            pontuacao += 10
            pontuacao_label["text"] = f"Pontuação: {pontuacao}"
            desativar_jogo()
            tocar_som("acerto.mp3")
        elif chute < numero_secreto:
            resultado["text"] = "Tente um número maior!"
            tocar_som("erro.mp3")
        else:
            resultado["text"] = "Tente um número menor!"
            tocar_som("erro.mp3")

        if tentativas == 0 and chute != numero_secreto:
            resultado["text"] = f"Você perdeu! Era {numero_secreto}."
            desativar_jogo()
            tocar_som("erro.mp3")

        tentativas_label["text"] = f"Tentativas: {tentativas}"
    except ValueError:
        resultado["text"] = "Digite um número válido."

def desativar_jogo():
    entry.config(state="disabled")
    botao.config(state="disabled")

# Variáveis globais
numero_secreto = 0
tentativas = 0
pontuacao = 0
limite_maximo = 100

# Janela
janela = tk.Tk()
janela.title("Jogo da Adivinhação - Versão com Dificuldade e Som")

titulo = tk.Label(janela, text="Escolha a dificuldade", font=("Arial", 14))
titulo.pack()

# Dificuldade
dificuldade_var = tk.StringVar(value="Médio")
tk.OptionMenu(janela, dificuldade_var, "Fácil", "Médio", "Difícil").pack()

tk.Button(janela, text="Começar Jogo", command=iniciar_jogo).pack(pady=5)

entry = tk.Entry(janela, font=("Arial", 12))
entry.pack()

botao = tk.Button(janela, text="Chutar", command=verificar_chute)
botao.pack()

resultado = tk.Label(janela, text="", font=("Arial", 12))
resultado.pack()

tentativas_label = tk.Label(janela, text="Tentativas:")
tentativas_label.pack()

pontuacao_label = tk.Label(janela, text="Pontuação: 0")
pontuacao_label.pack()

janela.mainloop()
