# MSYS2中使用pkg-config

1. 安装pkg-config和第三方库SDL2

```
pacman -S pkg-config mingw-w64-ucrt-x86_64-SDL2
```

2. 编写测试代码demo.c：

```c
#include <SDL2/SDL.h>
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

int main(int argc, char *argv[])
{
    SDL_Window *window = SDL_CreateWindow("demo", SDL_WINDOWPOS_CENTERED, SDL_WINDOWPOS_CENTERED, 640, 480, SDL_WINDOW_SHOWN);
    SDL_Renderer *renderer = SDL_CreateRenderer(window, -1, SDL_RENDERER_ACCELERATED);
    bool quit = false;
    SDL_Event event;

    SDL_SetRenderDrawColor(renderer, 255, 255, 255, 255);
    while (!quit)
    {
        while (SDL_PollEvent(&event))
        {
            switch (event.type)
            {
            case SDL_QUIT:
                quit = true;
                break;
            case SDL_MOUSEMOTION:
                if (SDL_GetMouseState(NULL, NULL) &SDL_BUTTON(1))
                    SDL_RenderDrawPoint(renderer, event.motion.x, event.motion.y);
                break;
            }
        }
        SDL_RenderPresent(renderer);
        SDL_Delay(1000 / 60);
    }
    SDL_DestroyWindow(window);
    SDL_DestroyRenderer(renderer);
    return 0;
}
```

3. 开始编译：

- 动态链接库

```
# 查看动态链接库
pkg-config SDL2 --libs
# 查看链接头文件
pkg-config SDL2 --cflags
# 编译
gcc demo.c `pkg-config SDL2 --cflags --libs`
```

- 静态链接库

```
# 查看静态链接库和头文件
pkg-config SDL2 --cflags --libs --static
# 编译
gcc demo.c `pkg-config SDL2 --cflags --libs --static` --static
```
