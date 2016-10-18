# PI-menu


#include <allegro5/allegro.h>
#include <allegro5/allegro_image.h>
#include <stdio.h>
#include <stdbool.h>

const int W = 640;
const int H = 480;




ALLEGRO_DISPLAY* janela = NULL;
ALLEGRO_EVENT_QUEUE* fila_eventos = NULL;
ALLEGRO_BITMAP *fundo = NULL;
ALLEGRO_BITMAP *fundo1 = NULL;
ALLEGRO_BITMAP *fundo2 = NULL;
ALLEGRO_BITMAP *fundo3 = NULL;
ALLEGRO_BITMAP *menu = NULL;
ALLEGRO_TIMER *tempo = NULL;
ALLEGRO_BITMAP *files[3];

bool init();

int main(){

    int i, r=0;

    init();
    files[0] = al_load_bitmap("menu1.png"); 
    files[1] = al_load_bitmap("menu2.png"); 
    files[2] = al_load_bitmap("menu3.png"); 
    files[3] = al_load_bitmap("menu4.png"); 



//------------------- Looping principal ----------------------
    while (1){
        ALLEGRO_EVENT evento;
        ALLEGRO_TIMEOUT timeout;
        ALLEGRO_KEYBOARD_STATE keystate;
        al_get_keyboard_state(&keystate);
        //al_register_event_source(fila_de_evento, al_get_timer_event_source(tempo));

        tempo = al_create_timer(1.0 / 60.0);
        al_start_timer(tempo);

        al_init_timeout(&timeout, 0.20);

        al_draw_bitmap(files[r], 0, 0, 0);
 
        
        int tem_eventos = al_wait_for_event_until(fila_eventos, &evento, &timeout);
 
        if (tem_eventos && evento.type == ALLEGRO_EVENT_DISPLAY_CLOSE)
        {
            break;
        }

        if(al_key_down(&keystate, ALLEGRO_KEY_DOWN)){
            if(++r > 3){
               r=0;}
            
        }

        if (al_key_down(&keystate, ALLEGRO_KEY_UP)){
            if(r-- <= 0)
                r=3;

        }

      
    

    al_flip_display(); 
    }    
    

//------------------- KILL tudo no fim do programa pra nao vazar memoria, rsrsrs ---------------
al_flip_display();
al_destroy_display(janela);
al_destroy_event_queue(fila_eventos);
al_destroy_bitmap(fundo);

for(i=0; i; i++){
al_destroy_bitmap(files[i]);
}

    return 0;
}	


//------------------- Inicia os caralho tudo ---------------------
bool init(){

    if (!al_init())
    {
        fprintf(stderr, "Falha ao inicializar a Allegro.\n");
        return false;
    }
 
    if (!al_init_image_addon())
    {
        fprintf(stderr, "Falha ao inicializar add-on allegro_image.\n");
        return false;
    }
 
    if (!al_install_keyboard())
    {
        fprintf(stderr, "Falha ao inicializar o teclado.\n");
        return false;
    }
 
    janela = al_create_display(W, H);
    if (!janela)
    {
        fprintf(stderr, "Falha ao criar janela.\n");
        return false;
    }
 
    al_set_window_title(janela, "Utilizando o Teclado");
 
    fila_eventos = al_create_event_queue();
    if (!fila_eventos)
    {
        fprintf(stderr, "Falha ao criar fila de eventos.\n");
        al_destroy_display(janela);
        return false;
    }
 
   /*fundo = al_load_bitmap("menu1.png"); 
    fundo1 = al_load_bitmap("menu2.png");
    fundo2 = al_load_bitmap("menu3.png");
    fundo3 = al_load_bitmap("menu4.png");
    if (!file[0] ||!file[1] ||!file[2]|| !file[3] )
    {
        fprintf(stderr, "Falha ao carregar imagem de fundo.\n");
        al_destroy_display(janela);
        al_destroy_event_queue(fila_eventos);
        return false;
    }*/
    al_register_event_source(fila_eventos, al_get_keyboard_event_source());
    al_register_event_source(fila_eventos, al_get_display_event_source(janela));
 
    return true;
}
