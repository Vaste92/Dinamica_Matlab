%Macchina ad un avvolgimento

%Tensione V continua impressa

%parametri della macchina

%elettrici
R=1;
V=10;
L1=0.02;
L2=0.01;
%meccanici
Cr=0;
b=0;
C0=0;
J=0.05;
k=0;
teta_s=0;

%dichiarazione funzioni
Lf=@(teta,L1,L2) L1+L2.*cos(2.*teta);
DLf=@(teta,L2) -2.*L2.*sin(2.*teta);
DIf=@(L,V,R,I,w,DL) (1./L).*(V-R.*I-I.*w.*DL); 
Dwf=@(w,Cem,Cl,J) (1./J).*(Cem-Cl);
Dtetaf=@(w) w;

%tempo
dt=0.0001;
t=0:dt:20;

%integrazione
n=length(t);
m=n+1;
L(1,m)=0;
DL(1,m)=0;
Cem(1,n)=0;
%V=sin(2.*pi.*t)+(1/3).*sin(6.*pi.*t)+(1/5).*sin(10.*pi.*t)+(1/7).*sin(14.*pi.*t);
M(3,m)=0; %riga 1: teta, riga 2: I, riga 3: w
M(2,1)=V(1,1)/R;
M(1,1)=-pi/4;
DM(3,m)=0; %riga 1: w, riga 2: DI, riga 3: DW

for i=1:n
    %valutazione funzioni
    L(1,i)=feval(Lf,M(1,i),L1,L2);
    DL(1,i)=feval(DLf,M(1,i),L2);
    Cm=C0+Cr*M(3,i)^2+k*(M(1,i)-teta_s);
    Cl=Cm+b*M(3,i);
    Cem(1,i)=DL(1,i)*M(2,i)^2*0.5;
    DM(1,i)=M(3,i);
    DM(2,i)=feval(DIf,L(1,i),V(1,1),R,M(2,i),M(3,i),DL(1,i));
    DM(3,i)=feval(Dwf,M(3,i),Cem(1,i),Cl,J);
    %integrazione a rettangoli
    M(:,i+1)=M(:,i)+DM(:,i)*dt;   
end;

%plot
figure(1);
plot(t,M(2,1:n));
figure(2);
plot((M(1,1:n)./pi).*180,Cem(1,1:n));
set(gcf, 'Renderer','OpenGL');
