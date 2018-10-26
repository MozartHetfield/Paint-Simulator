#include "stdafx.h"
#include <stdlib.h>
#include <stdio.h>
#define EPERM 1
#define ENOMEM 3
#define EINVAL 2
int** AllocMatrice ( int height, int width )
{
    int** Matrice;
    int i;
    Matrice = ( int** ) malloc ( height * sizeof ( int* ) );
    for ( i = 0; i < height; i++ )
    {
        Matrice[i] = ( int* ) calloc ( width, sizeof ( int ) );
    }
    if ( Matrice == NULL )
    {
        fprintf ( stderr, "%d\n", ENOMEM );
        exit ( EXIT_FAILURE );
    }
    return Matrice;
}
void FreeMatrice ( int** *Matrice, int height )
{
    int i;
    if ( *Matrice != NULL )
    {
        for ( i = 0; i < height; i++ )
        {
            if ( ( *Matrice ) [i] != NULL )
                free ( ( *Matrice ) [i] );
        }
        free ( *Matrice );
        *Matrice = NULL;
    }
}
void ReadMatrice ( int** PixelR, int** PixelG, int** PixelB, int height, int width )
{
    int i, j;
    for ( i = 0; i < height; i++ )
    {
        for ( j = 0; j < width; j++ )
        {
            scanf ( "%d", &PixelR[i][j] );
            scanf ( "%d", &PixelG[i][j] );
            scanf ( "%d", &PixelB[i][j] );
        }
    }
}
void WriteMatrice ( int** PixelR, int** PixelG, int** PixelB, int height, int width )
{
    int i, j;
    for ( i = 0; i < height; i++ )
    {
        for ( j = 0; j < width; j++ )
        {
            printf ( "%d ", PixelR[i][j] );
            printf ( "%d ", PixelG[i][j] );
            printf ( "%d", PixelB[i][j] );
            if ( j != ( width - 1 ) )
                printf ( " " );
        }
        printf ( "\n" );
    }
}
void CropMatrice ( int** *Matrice, int height, int width, int start_col, int start_line, int end_col, int end_line, int* heightReturn, int* widthReturn )
{
    int i, j;
    int height2 = 0;
    int width2 = 0;
    int** MatriceCropata;
    height2 = end_line - start_line + 1;
    width2 = end_col - start_col + 1;
    MatriceCropata = AllocMatrice ( height2, width2 );
    for ( i = 0; i < height2; i++ )
    {
        for ( j = 0; j < width2; j++ )
        {
            MatriceCropata[i][j] = ( *Matrice ) [i + start_line][j + start_col];
        }
    }
    FreeMatrice ( Matrice, height );
    *Matrice = AllocMatrice ( height2, width2 );
    for ( i = 0; i < height2; i++ )
    {
        for ( j = 0; j < width2; j++ )
        {
            ( *Matrice ) [i][j] = MatriceCropata[i][j];
        }
    }
    FreeMatrice ( &MatriceCropata, height2 );
    *heightReturn = height2;
    *widthReturn = width2;
}
int* AllocVector ( int height, int width )
{
    int* Vector;
    Vector = ( int* ) calloc ( height * width, sizeof ( int ) );
    if ( Vector == NULL )
    {
        fprintf ( stderr, "%d\n", ENOMEM );
        exit ( EXIT_FAILURE );
    }
    return Vector;
}
void FreeVector ( int * *Vector )
{
    if ( *Vector != NULL )
        free ( *Vector );
    *Vector = NULL;
}
int main()
{
    int cmd = 0, num_iter = 0, r = 0, g = 0, b = 0, num_rot = 0;
    int i = 0, j = 0, k = 0;
    int height = 0, width = 0, heightResize = 0, widthResize = 0, heightReturn = 0, widthReturn = 0;
    int start_col = 0, start_line = 0, end_col = 0, end_line = 0;
    int widthRotate = 0, heightRotate = 0, widthRotate2 = 0, heightRoeqweqwetate2 = 0;
    int** PixelR = NULL, **PixelG = NULL, **PixelB = NULL;
    int** MatriceResizeR = NULL, **MatriceResizeG = NULL, **MatriceResizeB = NULL;
    int** MatriceCopieR = NULL, **MatriceCopieG = NULL, **MatriceCopieB = NULL;
    int* VectorAjutor = NULL;

    scanf ( "%d", &cmd );
    if ( cmd == 1 )
    {
        FreeMatrice ( &PixelR, height );
        FreeMatrice ( &PixelG, height );
        FreeMatrice ( &PixelB, height );
        scanf ( "%d", &width ); //eroare->
        if ( width < 1 || width > 1024 )
        {
            fprintf ( stderr, "%d\n", EINVAL );
            FreeMatrice ( &PixelR, height );
            FreeMatrice ( &PixelG, height );
            FreeMatrice ( &PixelB, height );
            exit ( EXIT_FAILURE );
        }
        scanf ( "%d", &height ); //eroare ->
        if ( height < 1 || height > 1024 )
        {
            fprintf ( stderr, "%d\n", EINVAL );
            FreeMatrice ( &PixelR, height );
            FreeMatrice ( &PixelG, height );
            FreeMatrice ( &PixelB, height );
            exit ( EXIT_FAILURE );
        }
        PixelR = AllocMatrice ( height, width );
        PixelG = AllocMatrice ( height, width );
        PixelB = AllocMatrice ( height, width );
        ReadMatrice ( PixelR, PixelG, PixelB, height, width ); //eroare->
        for ( i = 0; i < height; i++ )
        {
            for ( j = 0; j < width; j++ )
            {
                if ( PixelR[i][j] < 0 || PixelR[i][j] > 255 || PixelG[i][j] < 0 || PixelG[i][j] > 255 || PixelB[i][j] < 0 || PixelB[i][j] > 255 )
                {
                    fprintf ( stderr, "%d\n", EINVAL );
                    FreeMatrice ( &PixelR, height );
                    FreeMatrice ( &PixelG, height );
                    FreeMatrice ( &PixelB, height );
                    exit ( EXIT_FAILURE );
                }
            }
        }
    }
    else
    {
        fprintf ( stderr, "%d\n", EPERM );
        FreeMatrice ( &PixelR, height );
        FreeMatrice ( &PixelG, height );
        FreeMatrice ( &PixelB, height );
        exit ( EXIT_FAILURE );
    }

    while ( 1 )
    {
        scanf ( "%d", &cmd );
        if ( cmd == 1 )
        {
            FreeMatrice ( &PixelR, height );
            FreeMatrice ( &PixelG, height );
            FreeMatrice ( &PixelB, height );
            scanf ( "%d", &width ); //eroare->
            if ( width < 1 || width > 1024 )
            {
                fprintf ( stderr, "%d\n", EINVAL );
                FreeMatrice ( &PixelR, height );
                FreeMatrice ( &PixelG, height );
                FreeMatrice ( &PixelB, height );
                exit ( EXIT_FAILURE );
            }
            scanf ( "%d", &height ); //eroare ->
            if ( height < 1 || height > 1024 )
            {
                fprintf ( stderr, "%d\n", EINVAL );
                FreeMatrice ( &PixelR, height );
                FreeMatrice ( &PixelG, height );
                FreeMatrice ( &PixelB, height );
                exit ( EXIT_FAILURE );
            }
            PixelR = AllocMatrice ( height, width );
            PixelG = AllocMatrice ( height, width );
            PixelB = AllocMatrice ( height, width );
            ReadMatrice ( PixelR, PixelG, PixelB, height, width ); //eroare->
            for ( i = 0; i < height; i++ )
            {
                for ( j = 0; j < width; j++ )
                {
                    if ( PixelR[i][j] < 0 || PixelR[i][j] > 255 || PixelG[i][j] < 0 || PixelG[i][j] > 255 || PixelB[i][j] < 0 || PixelB[i][j] > 255 )
                    {
                        fprintf ( stderr, "%d\n", EINVAL );
                        FreeMatrice ( &PixelR, height );
                        FreeMatrice ( &PixelG, height );
                        FreeMatrice ( &PixelB, height );
                        exit ( EXIT_FAILURE );
                    }
                }
            }
        }
        else if ( cmd == 2 )
        {
            scanf ( "%d", &start_col );
            scanf ( "%d", &start_line );
            scanf ( "%d", &end_col );
            scanf ( "%d", &end_line ); //error->
            if ( end_col < start_col || start_col < 0 || end_col >= width || end_line < start_line || start_line < 0 || end_line >= height )
            {
                fprintf ( stderr, "%d\n", EINVAL );
                FreeMatrice ( &PixelR, height );
                FreeMatrice ( &PixelG, height );
                FreeMatrice ( &PixelB, height );
                exit ( EXIT_FAILURE );
            }
            CropMatrice ( &PixelR, height, width, start_col, start_line, end_col, end_line, &heightReturn, &widthReturn );
            CropMatrice ( &PixelG, height, width, start_col, start_line, end_col, end_line, &heightReturn, &widthReturn );
            CropMatrice ( &PixelB, height, width, start_col, start_line, end_col, end_line, &heightReturn, &widthReturn );
            height = heightReturn;
            width = widthReturn;
        }
        else if ( cmd == 3 )
        {
            scanf ( "%d", &widthResize ); //eroare->
            if ( widthResize < 1 || widthResize > 1024 )
            {
                fprintf ( stderr, "%d\n", EINVAL );
                FreeMatrice ( &PixelR, height );
                FreeMatrice ( &PixelG, height );
                FreeMatrice ( &PixelB, height );
                exit ( EXIT_FAILURE );
            }
            scanf ( "%d", &heightResize ); //eroare->
            if ( heightResize < 1 || heightResize > 1024 )
            {
                fprintf ( stderr, "%d\n", EINVAL );
                FreeMatrice ( &PixelR, height );
                FreeMatrice ( &PixelG, height );
                FreeMatrice ( &PixelB, height );
                exit ( EXIT_FAILURE );
            }
            FreeMatrice ( &MatriceResizeR, heightResize );
            FreeMatrice ( &MatriceResizeG, heightResize );
            FreeMatrice ( &MatriceResizeB, heightResize );

            if ( widthResize < width && heightResize < height )
            {
                MatriceResizeR = AllocMatrice ( heightResize, widthResize );
                for ( i = 0; i < heightResize; i++ )
                {
                    for ( j = 0; j < widthResize; j++ )
                    {
                        MatriceResizeR[i][j] = PixelR[i][j];
                    }
                }
                FreeMatrice ( &PixelR, height );
                PixelR = AllocMatrice ( heightResize, widthResize );
                for ( i = 0; i < heightResize; i++ )
                {
                    for ( j = 0; j < widthResize; j++ )
                    {
                        PixelR[i][j] = MatriceResizeR[i][j];
                    }
                }
                FreeMatrice ( &MatriceResizeR, heightResize );

                MatriceResizeG = AllocMatrice ( heightResize, widthResize );
                for ( i = 0; i < heightResize; i++ )
                {
                    for ( j = 0; j < widthResize; j++ )
                    {
                        MatriceResizeG[i][j] = PixelG[i][j];
                    }
                }
                FreeMatrice ( &PixelG, height );
                PixelG = AllocMatrice ( heightResize, widthResize );
                for ( i = 0; i < heightResize; i++ )
                {
                    for ( j = 0; j < widthResize; j++ )
                    {
                        PixelG[i][j] = MatriceResizeG[i][j];
                    }
                }
                FreeMatrice ( &MatriceResizeG, heightResize );

                MatriceResizeB = AllocMatrice ( heightResize, widthResize );
                for ( i = 0; i < heightResize; i++ )
                {
                    for ( j = 0; j < widthResize; j++ )
                    {
                        MatriceResizeB[i][j] = PixelB[i][j];
                    }
                }
                FreeMatrice ( &PixelB, height );
                PixelB = AllocMatrice ( heightResize, widthResize );
                for ( i = 0; i < heightResize; i++ )
                {
                    for ( j = 0; j < widthResize; j++ )
                    {
                        PixelB[i][j] = MatriceResizeB[i][j];
                    }
                }
                FreeMatrice ( &MatriceResizeB, heightResize );

                height = heightResize;
                width = widthResize;
            }
            else if ( widthResize > width && heightResize > height )
            {
                MatriceResizeR = AllocMatrice ( heightResize, widthResize );
                for ( i = 0; i < height; i++ )
                {
                    for ( j = 0; j < width; j++ )
                    {
                        MatriceResizeR[i][j] = PixelR[i][j];
                    }
                }
                for ( i = height; i < heightResize; i++ )
                {
                    for ( j = 0; j < width; j++ )
                    {
                        MatriceResizeR[i][j] = 255;
                    }
                }
                for ( i = 0; i < heightResize; i++ )
                {
                    for ( j = width; j < widthResize; j++ )
                    {
                        MatriceResizeR[i][j] = 255;
                    }
                }
                FreeMatrice ( &PixelR, height );
                PixelR = AllocMatrice ( heightResize, widthResize );
                for ( i = 0; i < heightResize; i++ )
                {
                    for ( j = 0; j < widthResize; j++ )
                    {
                        PixelR[i][j] = MatriceResizeR[i][j];
                    }
                }
                FreeMatrice ( &MatriceResizeR, heightResize );

                MatriceResizeG = AllocMatrice ( heightResize, widthResize );
                for ( i = 0; i < height; i++ )
                {
                    for ( j = 0; j < width; j++ )
                    {
                        MatriceResizeG[i][j] = PixelG[i][j];
                    }
                }
                for ( i = height; i < heightResize; i++ )
                {
                    for ( j = 0; j < width; j++ )
                    {
                        MatriceResizeG[i][j] = 255;
                    }
                }
                for ( i = 0; i < heightResize; i++ )
                {
                    for ( j = width; j < widthResize; j++ )
                    {
                        MatriceResizeG[i][j] = 255;
                    }
                }
                FreeMatrice ( &PixelG, height );
                PixelG = AllocMatrice ( heightResize, widthResize );
                for ( i = 0; i < heightResize; i++ )
                {
                    for ( j = 0; j < widthResize; j++ )
                    {
                        PixelG[i][j] = MatriceResizeG[i][j];
                    }
                }
                FreeMatrice ( &MatriceResizeG, heightResize );

                MatriceResizeB = AllocMatrice ( heightResize, widthResize );
                for ( i = 0; i < height; i++ )
                {
                    for ( j = 0; j < width; j++ )
                    {
                        MatriceResizeB[i][j] = PixelB[i][j];
                    }
                }
                for ( i = height; i < heightResize; i++ )
                {
                    for ( j = 0; j < width; j++ )
                    {
                        MatriceResizeB[i][j] = 255;
                    }
                }
                for ( i = 0; i < heightResize; i++ )
                {
                    for ( j = width; j < widthResize; j++ )
                    {
                        MatriceResizeB[i][j] = 255;
                    }
                }
                FreeMatrice ( &PixelB, height );
                PixelB = AllocMatrice ( heightResize, widthResize );
                for ( i = 0; i < heightResize; i++ )
                {
                    for ( j = 0; j < widthResize; j++ )
                    {
                        PixelB[i][j] = MatriceResizeB[i][j];
                    }
                }
                FreeMatrice ( &MatriceResizeB, heightResize );

                height = heightResize;
                width = widthResize;
            }
            else if ( widthResize >= width && heightResize < height || widthResize > width && heightResize <= height )
            {
                MatriceResizeR = AllocMatrice ( heightResize, widthResize );
                for ( i = 0; i < heightResize; i++ )
                {
                    for ( j = 0; j < width; j++ )
                    {
                        MatriceResizeR[i][j] = PixelR[i][j];
                    }
                }
                FreeMatrice ( &PixelR, height );
                for ( i = 0; i < heightResize; i++ )
                {
                    for ( j = width; j < widthResize; j++ )
                    {
                        MatriceResizeR[i][j] = 255;
                    }
                }
                PixelR = AllocMatrice ( heightResize, widthResize );
                for ( i = 0; i < heightResize; i++ )
                {
                    for ( j = 0; j < widthResize; j++ )
                    {
                        PixelR[i][j] = MatriceResizeR[i][j];
                    }
                }
                FreeMatrice ( &MatriceResizeR, heightResize );

                MatriceResizeG = AllocMatrice ( heightResize, widthResize );
                for ( i = 0; i < heightResize; i++ )
                {
                    for ( j = 0; j < width; j++ )
                    {
                        MatriceResizeG[i][j] = PixelG[i][j];
                    }
                }
                FreeMatrice ( &PixelG, height );
                for ( i = 0; i < heightResize; i++ )
                {
                    for ( j = width; j < widthResize; j++ )
                    {
                        MatriceResizeG[i][j] = 255;
                    }
                }
                PixelG = AllocMatrice ( heightResize, widthResize );
                for ( i = 0; i < heightResize; i++ )
                {
                    for ( j = 0; j < widthResize; j++ )
                    {
                        PixelG[i][j] = MatriceResizeG[i][j];
                    }
                }
                FreeMatrice ( &MatriceResizeG, heightResize );

                MatriceResizeB = AllocMatrice ( heightResize, widthResize );
                for ( i = 0; i < heightResize; i++ )
                {
                    for ( j = 0; j < width; j++ )
                    {
                        MatriceResizeB[i][j] = PixelB[i][j];
                    }
                }
                FreeMatrice ( &PixelB, height );
                for ( i = 0; i < heightResize; i++ )
                {
                    for ( j = width; j < widthResize; j++ )
                    {
                        MatriceResizeB[i][j] = 255;
                    }
                }
                PixelB = AllocMatrice ( heightResize, widthResize );
                for ( i = 0; i < heightResize; i++ )
                {
                    for ( j = 0; j < widthResize; j++ )
                    {
                        PixelB[i][j] = MatriceResizeB[i][j];
                    }
                }
                FreeMatrice ( &MatriceResizeB, heightResize );

                height = heightResize;
                width = widthResize;
            }
            else if ( widthResize < width && heightResize >= height || widthResize <= width && heightResize > height )
            {
                MatriceResizeR = AllocMatrice ( heightResize, widthResize );
                for ( i = 0; i < height; i++ )
                {
                    for ( j = 0; j < widthResize; j++ )
                    {
                        MatriceResizeR[i][j] = PixelR[i][j];
                    }
                }
                FreeMatrice ( &PixelR, height );
                for ( i = 0; i < height; i++ )
                {
                    for ( j = width; j < widthResize; j++ )
                    {
                        MatriceResizeR[i][j] = 255;
                    }
                }
                PixelR = AllocMatrice ( heightResize, widthResize );
                for ( i = 0; i < heightResize; i++ )
                {
                    for ( j = 0; j < widthResize; j++ )
                    {
                        PixelR[i][j] = MatriceResizeR[i][j];
                    }
                }
                FreeMatrice ( &MatriceResizeR, heightResize );

                MatriceResizeG = AllocMatrice ( heightResize, widthResize );
                for ( i = 0; i < height; i++ )
                {
                    for ( j = 0; j < widthResize; j++ )
                    {
                        MatriceResizeG[i][j] = PixelG[i][j];
                    }
                }
                FreeMatrice ( &PixelG, height );
                for ( i = 0; i < height; i++ )
                {
                    for ( j = width; j < widthResize; j++ )
                    {
                        MatriceResizeG[i][j] = 255;
                    }
                }
                PixelG = AllocMatrice ( heightResize, widthResize );
                for ( i = 0; i < heightResize; i++ )
                {
                    for ( j = 0; j < widthResize; j++ )
                    {
                        PixelG[i][j] = MatriceResizeG[i][j];
                    }
                }
                FreeMatrice ( &MatriceResizeG, heightResize );

                MatriceResizeB = AllocMatrice ( heightResize, widthResize );
                for ( i = 0; i < height; i++ )
                {
                    for ( j = 0; j < widthResize; j++ )
                    {
                        MatriceResizeB[i][j] = PixelB[i][j];
                    }
                }
                FreeMatrice ( &PixelB, height );
                for ( i = 0; i < height; i++ )
                {
                    for ( j = width; j < widthResize; j++ )
                    {
                        MatriceResizeB[i][j] = 255;
                    }
                }
                PixelB = AllocMatrice ( heightResize, widthResize );
                for ( i = 0; i < heightResize; i++ )
                {
                    for ( j = 0; j < widthResize; j++ )
                    {
                        PixelB[i][j] = MatriceResizeB[i][j];
                    }
                }
                FreeMatrice ( &MatriceResizeB, heightResize );

                height = heightResize;
                width = widthResize;
            }
        }
        else if ( cmd == 4 )
        {
            scanf ( "%d", &start_col );
            scanf ( "%d", &start_line );
            scanf ( "%d", &end_col );
            scanf ( "%d", &end_line ); //error->
            if ( end_col < start_col || start_col < 0 || end_col >= width || end_line < start_line || start_line < 0 || end_line >= height )
            {
                fprintf ( stderr, "%d\n", EINVAL );
                FreeMatrice ( &PixelR, height );
                FreeMatrice ( &PixelG, height );
                FreeMatrice ( &PixelB, height );
                exit ( EXIT_FAILURE );
            }
            scanf ( "%d", &r );
            scanf ( "%d", &g );
            scanf ( "%d", &b ); //error->
            if ( r < 0 || r > 255 || g < 0 || g > 255 || b < 0 || b > 255 )
            {
                fprintf ( stderr, "%d\n", EINVAL );
                FreeMatrice ( &PixelR, height );
                FreeMatrice ( &PixelG, height );
                FreeMatrice ( &PixelB, height );
                exit ( EXIT_FAILURE );
            }

            for ( i = start_line; i <= end_line; i++ )
            {
                for ( j = start_col; j <= end_col; j++ )
                {
                    PixelR[i][j] = r;
                }
            }
            for ( i = start_line; i <= end_line; i++ )
            {
                for ( j = start_col; j <= end_col; j++ )
                {
                    PixelG[i][j] = g;
                }
            }
            for ( i = start_line; i <= end_line; i++ )
            {
                for ( j = start_col; j <= end_col; j++ )
                {
                    PixelB[i][j] = b;
                }
            }
        }
        else if ( cmd == 5 )
        {
            scanf ( "%d", &num_iter ); //error->
            if ( num_iter < 0 || num_iter > 2000 )
            {
                fprintf ( stderr, "%d\n", EINVAL );
                FreeMatrice ( &PixelR, height );
                FreeMatrice ( &PixelG, height );
                FreeMatrice ( &PixelB, height );
                exit ( EXIT_FAILURE );
            }
            if ( num_iter == 0 )
            {
                continue;
            }
            FreeMatrice ( &MatriceCopieR, height );
            FreeMatrice ( &MatriceCopieG, height );
            FreeMatrice ( &MatriceCopieB, height );
            MatriceCopieR = AllocMatrice ( height, width );
            MatriceCopieG = AllocMatrice ( height, width );
            MatriceCopieB = AllocMatrice ( height, width );
            for ( i = 0; i < height; i++ )
            {
                for ( j = 0; j < width; j++ )
                {
                    MatriceCopieR[i][j] = PixelR[i][j];
                }
            }
            for ( i = 0; i < height; i++ )
            {
                for ( j = 0; j < width; j++ )
                {
                    MatriceCopieG[i][j] = PixelG[i][j];
                }
            }
            for ( i = 0; i < height; i++ )
            {
                for ( j = 0; j < width; j++ )
                {
                    MatriceCopieB[i][j] = PixelB[i][j];
                }
            }

            for ( k = 0; k < num_iter; k++ )
            {
                for ( i = 0; i < height; i++ )
                {
                    for ( j = 0; j < width; j++ )
                    {
                        if ( i == 0 && j == 0 )
                            PixelR[0][0] = ( MatriceCopieR[0][1] + MatriceCopieR[1][0] ) / 2;
                        else if ( i == 0 && j == ( width - 1 ) )
                            PixelR[0][width - 1] = ( MatriceCopieR[0][width - 2] + MatriceCopieR[1][width - 1] ) / 2;
                        else if ( i == ( height - 1 ) && j == 0 )
                            PixelR[height - 1][0] = ( MatriceCopieR[height - 2][0] + MatriceCopieR[height - 1][1] ) / 2;
                        else if ( i == ( height - 1 ) && j == ( width - 1 ) )
                            PixelR[height - 1][width - 1] = ( MatriceCopieR[height - 1][width - 1] + MatriceCopieR[height - 1][width - 2] ) / 2;
                        else if ( i != 0 && i != ( height - 1 ) && j == 0 )
                            PixelR[i][0] = ( MatriceCopieR[i][1] + MatriceCopieR[i - 1][0] + MatriceCopieR[i + 1][0] ) / 3;
                        else if ( i != 0 && i != ( height - 1 ) && j == ( width - 1 ) )
                            PixelR[i][width - 1] = ( MatriceCopieR[i][width - 2] + MatriceCopieR[i - 1][width - 1] + MatriceCopieR[i + 1][width - 1] ) / 3;
                        else if ( i == 0 && j != 0 && j != ( width - 1 ) )
                            PixelR[0][j] = ( MatriceCopieR[0][j - 1] + MatriceCopieR[0][j + 1] + MatriceCopieR[1][j] ) / 3;
                        else if ( i == ( height - 1 ) && j != 0 && j != ( width - 1 ) )
                            PixelR[height - 1][j] = ( MatriceCopieR[height - 1][j - 1] + MatriceCopieR[height - 1][j + 1] + MatriceCopieR[height - 2][j] ) / 3;
                        else
                            PixelR[i][j] = ( MatriceCopieR[i][j - 1] + MatriceCopieR[i][j + 1] + MatriceCopieR[i - 1][j] + MatriceCopieR[i + 1][j] ) / 4;
                    }
                }
                for ( i = 0; i < height; i++ )
                {
                    for ( j = 0; j < width; j++ )
                    {
                        if ( i == 0 && j == 0 )
                            PixelG[0][0] = ( MatriceCopieG[0][1] + MatriceCopieG[1][0] ) / 2;
                        else if ( i == 0 && j == ( width - 1 ) )
                            PixelG[0][width - 1] = ( MatriceCopieG[0][width - 2] + MatriceCopieG[1][width - 1] ) / 2;
                        else if ( i == ( height - 1 ) && j == 0 )
                            PixelG[height - 1][0] = ( MatriceCopieG[height - 2][0] + MatriceCopieG[height - 1][1] ) / 2;
                        else if ( i == ( height - 1 ) && j == ( width - 1 ) )
                            PixelG[height - 1][width - 1] = ( MatriceCopieG[height - 1][width - 1] + MatriceCopieG[height - 1][width - 2] ) / 2;
                        else if ( i != 0 && i != ( height - 1 ) && j == 0 )
                            PixelG[i][0] = ( MatriceCopieG[i][1] + MatriceCopieG[i - 1][0] + MatriceCopieG[i + 1][0] ) / 3;
                        else if ( i != 0 && i != ( height - 1 ) && j == ( width - 1 ) )
                            PixelG[i][width - 1] = ( MatriceCopieG[i][width - 2] + MatriceCopieG[i - 1][width - 1] + MatriceCopieG[i + 1][width - 1] ) / 3;
                        else if ( i == 0 && j != 0 && j != ( width - 1 ) )
                            PixelG[0][j] = ( MatriceCopieG[0][j - 1] + MatriceCopieG[0][j + 1] + MatriceCopieG[1][j] ) / 3;
                        else if ( i == ( height - 1 ) && j != 0 && j != ( width - 1 ) )
                            PixelG[height - 1][j] = ( MatriceCopieG[height - 1][j - 1] + MatriceCopieG[height - 1][j + 1] + MatriceCopieG[height - 2][j] ) / 3;
                        else
                            PixelG[i][j] = ( MatriceCopieG[i][j - 1] + MatriceCopieG[i][j + 1] + MatriceCopieG[i - 1][j] + MatriceCopieG[i + 1][j] ) / 4;
                    }
                }
                for ( i = 0; i < height; i++ )
                {
                    for ( j = 0; j < width; j++ )
                    {
                        if ( i == 0 && j == 0 )
                            PixelB[0][0] = ( MatriceCopieB[0][1] + MatriceCopieB[1][0] ) / 2;
                        else if ( i == 0 && j == ( width - 1 ) )
                            PixelB[0][width - 1] = ( MatriceCopieB[0][width - 2] + MatriceCopieB[1][width - 1] ) / 2;
                        else if ( i == ( height - 1 ) && j == 0 )
                            PixelB[height - 1][0] = ( MatriceCopieB[height - 2][0] + MatriceCopieB[height - 1][1] ) / 2;
                        else if ( i == ( height - 1 ) && j == ( width - 1 ) )
                            PixelB[height - 1][width - 1] = ( MatriceCopieB[height - 1][width - 1] + MatriceCopieB[height - 1][width - 2] ) / 2;
                        else if ( i != 0 && i != ( height - 1 ) && j == 0 )
                            PixelB[i][0] = ( MatriceCopieB[i][1] + MatriceCopieB[i - 1][0] + MatriceCopieB[i + 1][0] ) / 3;
                        else if ( i != 0 && i != ( height - 1 ) && j == ( width - 1 ) )
                            PixelB[i][width - 1] = ( MatriceCopieB[i][width - 2] + MatriceCopieB[i - 1][width - 1] + MatriceCopieB[i + 1][width - 1] ) / 3;
                        else if ( i == 0 && j != 0 && j != ( width - 1 ) )
                            PixelB[0][j] = ( MatriceCopieB[0][j - 1] + MatriceCopieB[0][j + 1] + MatriceCopieB[1][j] ) / 3;
                        else if ( i == ( height - 1 ) && j != 0 && j != ( width - 1 ) )
                            PixelB[height - 1][j] = ( MatriceCopieB[height - 1][j - 1] + MatriceCopieB[height - 1][j + 1] + MatriceCopieB[height - 2][j] ) / 3;
                        else
                            PixelB[i][j] = ( MatriceCopieB[i][j - 1] + MatriceCopieB[i][j + 1] + MatriceCopieB[i - 1][j] + MatriceCopieB[i + 1][j] ) / 4;
                    }
                }

                for ( i = 0; i < height; i++ )
                {
                    for ( j = 0; j < width; j++ )
                    {
                        MatriceCopieR[i][j] = PixelR[i][j];
                    }
                }
                for ( i = 0; i < height; i++ )
                {
                    for ( j = 0; j < width; j++ )
                    {
                        MatriceCopieG[i][j] = PixelG[i][j];
                    }
                }
                for ( i = 0; i < height; i++ )
                {
                    for ( j = 0; j < width; j++ )
                    {
                        MatriceCopieB[i][j] = PixelB[i][j];
                    }
                }

            }
            FreeMatrice ( &MatriceCopieR, height );
            FreeMatrice ( &MatriceCopieG, height );
            FreeMatrice ( &MatriceCopieB, height );
        }
        else if ( cmd == 6 )
        {
            scanf ( "%d", &num_rot ); //error->
            if ( num_rot < 1 || num_rot > 3 )
            {
                fprintf ( stderr, "%d\n", EINVAL );
                FreeMatrice ( &PixelR, height );
                FreeMatrice ( &PixelG, height );
                FreeMatrice ( &PixelB, height );
                exit ( EXIT_FAILURE );
            }

            FreeVector ( &VectorAjutor );
            heightRotate = width;
            widthRotate = height;

            k = 0;
            VectorAjutor = AllocVector ( heightRotate, widthRotate );
            for ( j = 0; j < width; j++ )
            {
                for ( i = ( height - 1 ); i >= 0; i-- )
                {
                    VectorAjutor[k] = PixelR[i][j];
                    k++;
                }
            }
            FreeMatrice ( &PixelR, height );
            PixelR = AllocMatrice ( heightRotate, widthRotate );
            k = 0;
            for ( i = 0; i < heightRotate; i++ )
            {
                for ( j = 0; j < widthRotate; j++ )
                {
                    PixelR[i][j] = VectorAjutor[k];
                    k++;
                }
            }
            FreeVector ( &VectorAjutor );

            k = 0;
            VectorAjutor = AllocVector ( heightRotate, widthRotate );
            for ( j = 0; j < width; j++ )
            {
                for ( i = ( height - 1 ); i >= 0; i-- )
                {
                    VectorAjutor[k] = PixelG[i][j];
                    k++;
                }
            }
            FreeMatrice ( &PixelG, height );
            PixelG = AllocMatrice ( heightRotate, widthRotate );
            k = 0;
            for ( i = 0; i < heightRotate; i++ )
            {
                for ( j = 0; j < widthRotate; j++ )
                {
                    PixelG[i][j] = VectorAjutor[k];
                    k++;
                }
            }
            FreeVector ( &VectorAjutor );

            k = 0;
            VectorAjutor = AllocVector ( heightRotate, widthRotate );
            for ( j = 0; j < width; j++ )
            {
                for ( i = ( height - 1 ); i >= 0; i-- )
                {
                    VectorAjutor[k] = PixelB[i][j];
                    k++;
                }
            }
            FreeMatrice ( &PixelB, height );
            PixelB = AllocMatrice ( heightRotate, widthRotate );
            k = 0;
            for ( i = 0; i < heightRotate; i++ )
            {
                for ( j = 0; j < widthRotate; j++ )
                {
                    PixelB[i][j] = VectorAjutor[k];
                    k++;
                }
            }
            FreeVector ( &VectorAjutor );

            if ( num_rot == 1 )
            {
                height = heightRotate;
                width = widthRotate;
            }
            if ( num_rot > 1 )
            {
                VectorAjutor = AllocVector ( height, width );
                k = 0;
                for ( j = 0; j < widthRotate; j++ )
                {
                    for ( i = ( heightRotate - 1 ); i >= 0; i-- )
                    {
                        VectorAjutor[k] = PixelR[i][j];
                        k++;
                    }
                }
                FreeMatrice ( &PixelR, heightRotate );
                PixelR = AllocMatrice ( height, width );
                k = 0;
                for ( i = 0; i < height; i++ )
                {
                    for ( j = 0; j < width; j++ )
                    {
                        PixelR[i][j] = VectorAjutor[k];
                        k++;
                    }
                }
                FreeVector ( &VectorAjutor );

                VectorAjutor = AllocVector ( height, width );
                k = 0;
                for ( j = 0; j < widthRotate; j++ )
                {
                    for ( i = ( heightRotate - 1 ); i >= 0; i-- )
                    {
                        VectorAjutor[k] = PixelG[i][j];
                        k++;
                    }
                }
                FreeMatrice ( &PixelG, heightRotate );
                PixelG = AllocMatrice ( height, width );
                k = 0;
                for ( i = 0; i < height; i++ )
                {
                    for ( j = 0; j < width; j++ )
                    {
                        PixelG[i][j] = VectorAjutor[k];
                        k++;
                    }
                }
                FreeVector ( &VectorAjutor );

                VectorAjutor = AllocVector ( height, width );
                k = 0;
                for ( j = 0; j < widthRotate; j++ )
                {
                    for ( i = ( heightRotate - 1 ); i >= 0; i-- )
                    {
                        VectorAjutor[k] = PixelB[i][j];
                        k++;
                    }
                }
                FreeMatrice ( &PixelB, heightRotate );
                PixelB = AllocMatrice ( height, width );
                k = 0;
                for ( i = 0; i < height; i++ )
                {
                    for ( j = 0; j < width; j++ )
                    {
                        PixelB[i][j] = VectorAjutor[k];
                        k++;
                    }
                }
                FreeVector ( &VectorAjutor );
            }
            if ( num_rot > 2 )
            {
                k = 0;
                VectorAjutor = AllocVector ( heightRotate, widthRotate );
                for ( j = 0; j < width; j++ )
                {
                    for ( i = ( height - 1 ); i >= 0; i-- )
                    {
                        VectorAjutor[k] = PixelR[i][j];
                        k++;
                    }
                }
                FreeMatrice ( &PixelR, height );
                PixelR = AllocMatrice ( heightRotate, widthRotate );
                k = 0;
                for ( i = 0; i < heightRotate; i++ )
                {
                    for ( j = 0; j < widthRotate; j++ )
                    {
                        PixelR[i][j] = VectorAjutor[k];
                        k++;
                    }
                }
                FreeVector ( &VectorAjutor );

                k = 0;
                VectorAjutor = AllocVector ( heightRotate, widthRotate );
                for ( j = 0; j < width; j++ )
                {
                    for ( i = ( height - 1 ); i >= 0; i-- )
                    {
                        VectorAjutor[k] = PixelG[i][j];
                        k++;
                    }
                }
                FreeMatrice ( &PixelG, height );
                PixelG = AllocMatrice ( heightRotate, widthRotate );
                k = 0;
                for ( i = 0; i < heightRotate; i++ )
                {
                    for ( j = 0; j < widthRotate; j++ )
                    {
                        PixelG[i][j] = VectorAjutor[k];
                        k++;
                    }
                }
                FreeVector ( &VectorAjutor );

                k = 0;
                VectorAjutor = AllocVector ( heightRotate, widthRotate );
                for ( j = 0; j < width; j++ )
                {
                    for ( i = ( height - 1 ); i >= 0; i-- )
                    {
                        VectorAjutor[k] = PixelB[i][j];
                        k++;
                    }
                }
                FreeMatrice ( &PixelB, height );
                PixelB = AllocMatrice ( heightRotate, widthRotate );
                k = 0;
                for ( i = 0; i < heightRotate; i++ )
                {
                    for ( j = 0; j < widthRotate; j++ )
                    {
                        PixelB[i][j] = VectorAjutor[k];
                        k++;
                    }
                }
                FreeVector ( &VectorAjutor );

                height = heightRotate;
                width = widthRotate;
            }
        }
        else if ( cmd == 7 )
        {
            scanf ( "%d", &start_col );
            scanf ( "%d", &start_line ); //error->
            if ( 0 > start_line || start_line >= height || 0 > start_col || start_col >= width )
            {
                fprintf ( stderr, "%d\n", EINVAL );
                FreeMatrice ( &PixelR, height );
                FreeMatrice ( &PixelG, height );
                FreeMatrice ( &PixelB, height );
                exit ( EXIT_FAILURE );
            }
            scanf ( "%d", &r );
            scanf ( "%d", &g );
            scanf ( "%d", &b ); //error->
            if ( r < 0 || r > 255 || g < 0 || g > 255 || b < 0 || b > 255 )
            {
                fprintf ( stderr, "%d\n", EINVAL );
                FreeMatrice ( &PixelR, height );
                FreeMatrice ( &PixelG, height );
                FreeMatrice ( &PixelB, height );
                exit ( EXIT_FAILURE );
            }
        }
        else if ( cmd == 8 )
        {
            printf ( "%d ", width );
            printf ( "%d\n", height );
            WriteMatrice ( PixelR, PixelG, PixelB, height, width );
        }
        else if ( cmd == 0 )
        {
            break;
        }
        else
        {
            fprintf ( stderr, "%d\n", EPERM );
            FreeMatrice ( &PixelR, height );
            FreeMatrice ( &PixelG, height );
            FreeMatrice ( &PixelB, height );
            exit ( EXIT_FAILURE );
        }
    }//<- error
    return 0;
}
