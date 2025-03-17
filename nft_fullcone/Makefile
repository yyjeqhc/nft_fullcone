obj-m += nft_fullcone.o

nft_fullcone-y := nft_ext_fullcone.o nf_nat_fullcone.o


all:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules

clean:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
