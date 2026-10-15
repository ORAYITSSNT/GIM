% traitement automatique d'une réponse à l'échelon

close all

sortie=donnees(:,2);

% détermination de la valeur finale
VF=sortie(end);
% création des vecteurs à 95% et 105% pour l'affichage de la zone de mesure
VF95=0.95*VF*ones(size(temps));
VF105=1.05*VF*ones(size(temps));

% détermintation des intersection entre les barres à 95% et 105% et la
% sortie

% seuil de détection du temps de réponse à 5%
seuil=1e-3;

t5=[];
i=0;

while (isempty(t5) && i<10)
t95=find(abs(sortie-VF95)<seuil);
t105=find(abs(sortie-VF105)<seuil);
% calcul du temps de réponse à 5%
t5=max([t95;t105]);
% augmentation du seuil si non détection
seuil=seuil*2;
% augmentation du compteur pour limiter le nombre de boucles while
i=i+1;
end


% affichage
plot(temps,donnees,temps,VF95,temps,VF105,temps(t95),sortie(t95),'+',temps(t105),sortie(t105),'+',temps(t5),sortie(t5),'*')

disp('Valeur Finale :')
VF
if (isempty(t5))
    disp('Temps de réponse non trouvé')
else
    disp('Temps de réponse à 5% :')
    temps(t5)
end
disp('Dépassement')
(max(sortie)-VF)/VF*100